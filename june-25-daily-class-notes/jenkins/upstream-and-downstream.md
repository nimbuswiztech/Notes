# Upstream and Downstream

### **What is Upstream and Downstream in Jenkins?**

In Jenkins, **Upstream** and **Downstream** jobs define the **build dependency flow** between Jenkins jobs (projects/pipelines):

```
job-A  -->  Job-B
 ↑          ↓
Upstream   Downstream
```

***

### 📘 **Definitions**

#### 🔹 Upstream Job:

A job that **triggers another job** after it completes.

#### 🔹 Downstream Job:

A job that is **triggered** after another job finishes.

***

### 🏗️ Example Scenario (Real-Time)

Let’s say you have a DevOps pipeline:

1. **Job-A**: Checkout code from GitHub
2. **Job-B**: Build the application
3. **Job-C**: Deploy to development environment
4. **Job-D**: Run automated tests

Here:

* `Job-A` is **upstream** to `Job-B`
* `Job-B` is **downstream** of `Job-A`
* `Job-B` is **upstream** to `Job-C`
* `Job-D` is **downstream** of `Job-C`

***

### 📈 Real-World Analogy

> Think of a **conveyor belt** in a factory.

* **Upstream**: The machines earlier in the line (e.g., raw material prep)
* **Downstream**: The machines later in the line (e.g., packaging)

If something breaks upstream, downstream cannot proceed.

***

### ✅ How to Set Up Upstream/Downstream in Jenkins

#### 🔸 Option 1: **Using Freestyle Projects (Post-Build Actions)**

1. Go to **Job-A → Configure**
2. Scroll to **Post-build Actions**
3. Click **“Add post-build action” → “Build other projects”**
4. Enter: `Job-B`
5. Save

🔁 Now whenever `Job-A` runs successfully, it will **trigger `Job-B` (downstream)**.

***

#### 🔸 Option 2: **Using Build Triggers**

1. Go to **Job-B → Configure**
2. Scroll to **Build Triggers**
3. Check ✅ **Build after other projects are built**
4. Enter: `Job-A`
5. Save

🔁 This means `Job-B` will be triggered after `Job-A` (its upstream) completes.

***

#### 🔸 Option 3: **Pipeline Jobs**

In **Declarative Pipeline**:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building..."
            }
        }
        stage('Trigger Downstream') {
            steps {
                build job: 'Job-B'
            }
        }
    }
}
```

This makes `Job-B` downstream of this pipeline.

***

### 🔍 View Upstream/Downstream Links in Jenkins

Jenkins automatically shows these links on the **job dashboard**:

* **“Upstream Projects”**: Shows which jobs trigger this one
* **“Downstream Projects”**: Shows which jobs are triggered after this

Useful for visualizing job chains.

***

### 🎯 Real-Time Use Cases

#### 1. **Microservices Deployment**

* Job-A: Build Docker image for `Service-A`
* Job-B: Push image to ECR (downstream)
* Job-C: Deploy to Kubernetes (downstream of B)

#### 2. **Test Automation Pipeline**

* Job-A: Build app
* Job-B: Run unit tests
* Job-C: Run integration tests (triggered only after unit tests succeed)

***

### 🧑‍🏫 Teaching Tips

* Show students how to create 2 simple jobs: `Job-A` and `Job-B`
* Add “echo” or print statements to distinguish jobs
* Use **post-build trigger** to link Job-A → Job-B
* Let them run `Job-A` and observe how `Job-B` starts automatically

***

Would you like me to give a **demo script** or Freestyle + Pipeline template for upstream/downstream chaining?

**You said:**

Trigger only if build is stable Trigger even if the build is unstable Trigger even if the build fails Always trigger, even if the build is aborted

**ChatGPT said:**

Great question, Siddesh!

When setting **upstream/downstream relationships** in Jenkins (especially in **Freestyle jobs**), Jenkins gives you several options on **when** to trigger a downstream job based on the **result of the upstream build**.

Let’s explain each option clearly with real-time DevOps examples + teaching tips.

***

### ✅ **Trigger Conditions for Downstream Jobs**

These options are available when you configure:

> **Job-A → Post-build Actions → Build other projects**

You’ll see a dropdown like:

```
Trigger only if build is stable
Trigger even if the build is unstable
Trigger even if the build fails
Always trigger, even if the build is aborted
```

***

#### 🔹 1. **Trigger only if build is stable (default)**

**💡 Meaning:**

Downstream job will run **only if the upstream job finished successfully (exit status 0)**.

**📘 Use Case:**

In a CI/CD pipeline:

* **Job-A** builds the code
* **Job-B** runs automated tests

👉 You don’t want to run tests unless the build is successfull

***

#### 🔹 2. **Trigger even if the build is unstable**

**💡 Meaning:**

Triggers downstream even if the upstream job is **unstable** (e.g., test failures), but not if it fails.

Jenkins marks a build as unstable if:

* It completed with warnings
* Unit tests failed but the build technically compiled

**📘 Use Case:**

* Run test reports or send notifications even if some tests fail
* Maybe deploy to dev even on minor issues

**🧪 Demo:**

1. Use JUnit plugin or shell script that exits with 0 but simulates test warnings
2. Mark build as unstable
3. Trigger Job-B → It will run even though Job-A is unstable

***

#### 🔹 3. **Trigger even if the build fails**

**💡 Meaning:**

Downstream job will run **even if upstream job fails completely (exit 1 or any error)**.

**📘 Use Case:**

* Send failure report
* Clean up environment
* Rollback previous deployment

**🧪 Teaching Example:**

1. In Job-A → Add `exit 1`
2. In Job-B → Just `echo "cleanup or report"`
3. Select: "Trigger even if the build fails"
4. Run Job-A → Fails → Job-B still runs

***

#### 🔹 4. **Always trigger, even if the build is aborted**

**💡 Meaning:**

No matter what happens in Job-A (success, unstable, failure, or even **manually aborted**) — Jenkins will trigger Job-B.

**📘 Use Case:**

* Always run cleanup jobs
* Always send audit logs or Slack notifications

**🧪  Demo:**

1. Create Job-A with `sleep 60` (long job)
2. Start it, then **abort** manually
3. Job-B will still run if this option is selected

***
