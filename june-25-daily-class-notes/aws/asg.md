# ASG

### Brief Introduction to AWS ASG

Start the session by introducing the concept of **Auto Scaling Groups**:

* **Definition:** An ASG in AWS automatically manages the number of EC2 instances according to demand, enabling applications to scale in and out based on set policies.
* **Components:**
  * **Launch Template/Configuration:** Blueprint for EC2 instances (AMI, type, security groups, etc.)
  * **Scaling Policies:** Rules for adding/removing instances (target tracking, step scaling, schedule-based)
  * **Health Checks:** Mechanism to replace unhealthy instances.
  * **Load Balancer (optional):** Evenly distributes traffic among instances.
  * **Desired, Minimum, Maximum Capacity:** Controls the flexibility of scaling.

### Real-Time Practical Scenario

**Scenario:** “You’re running a web application that experiences increased traffic during business hours and reduced load during nights and weekends.”

### Hands-on Lab Steps

1. **Set up a Launch Template**
   * Choose an Amazon Machine Image (AMI) (e.g., Amazon Linux).
   * Define instance type (t2.micro for demo).
   * Attach key pair for SSH access.
2. **Create a Target Group and an Application Load Balancer (ALB)**
   * Target group will receive instances launched by ASG.
   * ALB will route incoming HTTP/HTTPS traffic to those instances.
3. **Create an Auto Scaling Group**
   * Associate with launch template and target group.
   * Set:
     * Minimum: 1
     * Maximum: 4
     * Desired capacity: 2
4. **Configure Scaling Policies**
   * **Target Tracking:** Maintain average CPU at 50%.
   * **Step Scaling:** Add 1 instance if CPU >70% for 5 minutes, remove 1 if <30%.
5. **Simulate Traffic and Scaling**
   * Use a tool like Apache Benchmark or scripts to generate load.
   * Observe instance scale-out when CPU spikes; scale-in when load drops.
6. **Demonstrate Fault Tolerance**
   * Manually stop an EC2 instance from the ASG and show that ASG will replace it to maintain desired capacity.

**Bonus:** Set up scheduled scaling to increase capacity during specific hours (e.g., scale to 4 from 9 AM - 6 PM, and down to 1 otherwise).

### Real-World Examples

* **E-Commerce Sites:** Scale up for seasonal sales and scale down post-sale to save cost.
* **News Portals:** Quickly handle surges during breaking news.
* **SaaS Applications:** Match user demand across global time zones.

### &#x20;Best Practices to Share

* Always configure health checks (EC2 and ELB) for accurate recovery.
* Use launch templates for consistency and versioning.
* Test scaling policies periodically.
* Enable notifications (SNS) for scaling events.

## Capacity

* **Desired Capacity:** This is the ideal number of EC2 instances you want running in your ASG at any given time. The ASG tries to maintain this number under normal conditions. When scaling activities are triggered (like a scheduled change or CloudWatch alarm), the desired capacity is adjusted up or down.
* **Minimum Capacity:** This sets the **lowest number of EC2 instances** that your ASG will maintain. Even if there's no demand, the ASG will not terminate instances below this number. It ensures you always have a minimum set of resources running (for example, to guarantee application availability).
* **Maximum Capacity:** This is the **highest number of EC2 instances** that your ASG can launch. No scaling policies or manual actions will increase the group beyond this limit, preventing unexpected surges that could cause high costs or stress downstream systems.

**Example:**\
If you set:

* Minimum: 2
* Desired: 3
* Maximum: 5

Your ASG will launch 3 instances by default, never letting the count fall below 2, and never letting it increase above 5—no matter what scaling event occurs.

This configuration helps you control availability, performance, and cost of your workloads by automatically adjusting compute resources within safe boundaries.

Scaling  : You can resize your Auto Scaling group manually or automatically to meet changes in demand. Scaling limits Set limits on how much your desired capacity can be increased or decreased. Min desired capacity Equal or less than desired capacity Max desired capacity Equal or greater than desired capacity Automatic scaling - optional Choose whether to use a target tracking policy\[Info]\() You can set up other metric-based scaling policies and scheduled scaling after creating your Auto Scaling group. No scaling policiesYour Auto Scaling group will remain at its initial size and will not dynamically resize to meet demand. Target tracking scaling policyChoose a CloudWatch metric and target value and let the scaling policy adjust the desired capacity in proportion to the metric's value.Scaling  \[Info]\() You can resize your Auto Scaling group manually or automatically to meet changes in demand. Scaling limits Set limits on how much your desired capacity can be increased or decreased. Min desired capacity Equal or less than desired capacity Max desired capacity Equal or greater than desired capacity Automatic scaling - optional Choose whether to use a target tracking policy\[Info]\() You can set up other metric-based scaling policies and scheduled scaling after creating your Auto Scaling group. No scaling policiesYour Auto Scaling group will remain at its initial size and will not dynamically resize to meet demand. Target tracking scaling policyChoose a CloudWatch metric and target value and let the scaling policy adjust the desired capacity in proportion to the metric's value.

### Understanding Scaling in AWS Auto Scaling Groups (ASG)

When configuring an **Auto Scaling Group (ASG)** in AWS, you control how your compute resources automatically adjust to dynamic workloads using scaling parameters and policies. Here’s a breakdown of the concepts in your prompt:

### Manual vs. Automatic Scaling

* **Manual scaling:** You adjust the number of instances yourself (change the desired capacity up or down). The ASG won’t change automatically based on workload demand.
* **Automatic scaling:** The ASG adjusts the number of instances in response to monitored metrics or schedules, keeping your application responsive and cost-efficient.

### Scaling Limits: Minimum, Desired, Maximum Capacities

* **Minimum Desired Capacity:** The lowest number of instances your ASG will maintain. If demand drops, the group never scales below this value.
* **Maximum Desired Capacity:** The highest number of instances your ASG can launch. Even if demand spikes sharply, it cannot exceed this upper limit.
* **Desired Capacity:** The current, actively maintained number of instances, which the ASG tries to match unless scaling events trigger a change.

> **Rule:**
>
> * The **minimum desired capacity** must be _equal to or less_ than the current desired capacity.
> * The **maximum desired capacity** must be _equal to or greater_ than the current desired capacity.
> * The desired capacity stays within the min-max range.



### Key Concepts

* **Desired Capacity**: Number of instances ASG tries to maintain.
* **Min/Max Size**: Limits for instance count.
* **Warmup/Cooldown**: Settings to manage how quickly scaling actions repeat.
* **Health Checks**: ASG replaces unreachable/unhealthy instances automatically.

### Example

Suppose you want to keep your CPU utilization close to 50%, but during promotions, expect large traffic spikes:

* Use _target tracking_ to maintain 50% CPU normally.
* Add a _step scaling_ policy to aggressively add instances if CPU exceeds 80% for quicker scaling during traffic spikes.

### Automatic Scaling Policies

* **No scaling policies:** The ASG remains static at its initial size, regardless of load.
* **Target Tracking Scaling Policy:** You select a **CloudWatch metric** (like average CPU utilization) and set a target value (e.g., 50%).\
  The ASG will automatically add or remove instances to keep the metric close to your target.
  * For example, if you set a target of 50% CPU, and CPU usage rises, the ASG increases capacity (more instances) to reduce load; if usage drops, the group scales in to save cost.
* **Other scaling options** (configured after creation):
  * **Step scaling:** Responds to metric thresholds with defined actions.
  * **Scheduled scaling:** Changes desired capacity at set times (e.g., scale up at 9am, down at 9pm).



### &#x20;Scenario-Based Questions for Students

1. **You have a web app that peaks between 6–10 PM daily. How would you configure an ASG to save costs outside peak hours?**
2. **An ASG is scaling out, but traffic is not evenly distributed. What could be wrong?**
3. **If the health check type is set to EC2, but the underlying web service fails, will ASG replace the instance? Why or why not?**
4. **Your ASG doesn’t scale in after load decreases. What checks would you perform?**
5. **How would you integrate a custom alarm (e.g., queue backlog) as a scaling trigger for an ASG?**
6. **Describe how you’d recover quickly if AWS announces scheduled maintenance on your availability zone.**
