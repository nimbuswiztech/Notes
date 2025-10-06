# Possible cross questions

**Common Cross-Questions for 3-Tier Architecture Interview**

**Technical Deep-Dive Questions:**

**1. "Walk me through what happens when a user clicks 'Add to Cart' in your e-commerce setup"**\
Yeah, so when someone clicks that button, the React frontend first validates the item locally, then sends a POST request to our Node.js API. The API checks if the user is logged in, validates the product ID, calls the inventory service to make sure we have stock, then updates the cart in Redis for quick access and also saves it to PostgreSQL for persistence. The whole flow takes maybe 200-300ms if everything's running smooth.

**2. "How do you handle database connection pooling across multiple application servers?"**\
We use PgBouncer as our connection pooler. Each app server connects to PgBouncer instead of directly to the database. I configured it with a pool size of 25 connections per database, and set max client connections to 1000. This way, even if we have 10 app servers, we're not overwhelming the database with too many connections. We also set connection timeouts and retry logic in the application code.

**3. "What happens if your Redis cache goes down?"**\
If Redis goes down, the app falls back to the database for cart data. We have error handling in the code that catches Redis failures and automatically queries PostgreSQL instead. Performance takes a hit, but the site keeps working. We also have Redis configured in a cluster with replication, so usually only one node fails at a time.

**4. "How do you ensure data consistency between your MongoDB and PostgreSQL databases?"**\
That's a good question. We use an event-driven approach with RabbitMQ. When something changes in PostgreSQL - like an order status - we publish an event to RabbitMQ. The MongoDB services listen for these events and update their collections accordingly. It's eventually consistent, not immediately, but for product catalogs that's usually fine.

**5. "Explain your deployment strategy. How do you deploy without downtime?"**\
We use blue-green deployments. We have two identical environments - blue and green. When we deploy, we update the green environment while blue is serving traffic. Once green is ready and passes health checks, we switch the load balancer to point to green. If something goes wrong, we can switch back to blue immediately.

**Scaling and Performance Questions:**

**6. "How would you handle a sudden 10x traffic spike?"**\
Our auto-scaling kicks in when CPU hits 70%. But for a 10x spike, that might not be fast enough. We'd probably need to pre-warm some instances or use predictive scaling. For the database, we have read replicas that can handle the extra load. The CDN would help with static content. But honestly, a 10x spike would probably cause some slowdown until the scaling catches up.

**7. "Why did you choose EKS over running containers on plain EC2?"**\
EKS handles a lot of the heavy lifting - like scaling, service discovery, and load balancing. With plain EC2, I'd have to manage all that myself. EKS also gives us better integration with AWS services and makes it easier to update and patch the cluster. The tradeoff is cost - EKS is more expensive than managing your own containers.

**8. "How do you monitor performance across all three tiers?"**\
I use Prometheus to collect metrics from all layers. Each tier exposes metrics - response times, error rates, resource usage. Grafana visualizes everything in dashboards. For the database, I monitor connection counts, query performance, and replication lag. For the application, it's mostly response times and error rates. The load balancer gives us traffic patterns.

**Problem-Solving Questions:**

**9. "A user reports that their shopping cart is empty after logging in. How do you troubleshoot?"**\
First, I'd check if it's happening to other users or just this one. If it's just one user, I'd look at the Redis cache to see if their cart data is there. If not, I'd check PostgreSQL. I'd also look at the application logs to see if there were any errors during their session. Could be a cookie issue, a Redis expiration problem, or maybe they logged in from a different device.

**10. "Your database is running at 90% CPU. What's your immediate action?"**\
First, I'd check what queries are running and if there's a slow query causing the spike. I'd look at the query logs and see if we can optimize anything. Short term, I might add more read replicas to spread the load. Long term, I'd look at indexing, query optimization, or maybe partitioning if it's a large table.

**11. "How would you migrate your current setup to a different cloud provider?"**\
I'd start with the database - export data and import it to the new cloud. For the application, since everything's containerized, it should be pretty portable. The tricky part would be the networking and load balancers since those are cloud-specific. I'd probably do it in phases - maybe start with a dev environment first, then staging, then production.

**Security Questions:**

**12. "How do you secure communication between your tiers?"**\
All internal communication uses HTTPS with TLS 1.3. The database subnets have no internet access - they can only be reached from the application tier. We use security groups to restrict which services can talk to each other. API keys and tokens are stored in AWS Secrets Manager, not hardcoded in the application.

**13. "What happens if someone tries to inject SQL into your application?"**\
Our application uses parameterized queries, so SQL injection shouldn't work. We also have input validation on the frontend and backend. Plus, we run security scans with tools like SonarQube as part of our CI/CD pipeline. If someone did try it, we'd see it in the logs and our monitoring would catch unusual database activity.

**Cost and Optimization Questions:**

**14. "How do you optimize costs in this setup?"**\
We use spot instances for non-critical workloads. The auto-scaling makes sure we're not running more servers than needed. We also use Reserved Instances for predictable workloads. For storage, we use different storage classes - frequently accessed data on SSD, older data on cheaper storage. The CDN reduces bandwidth costs too.

**15. "Would you consider serverless for any part of this architecture?"**\
Maybe for background jobs or APIs that don't get much traffic. Lambda could handle things like image processing or sending emails. But for the main application, containers give us more control and better performance. Serverless has cold start issues that might hurt user experience.

These questions test your practical knowledge beyond just knowing the theory. The key is to answer based on real experience and show you understand the tradeoffs involved in each decision.

⁂
