# Build Triggers

### **Different Types of Build Triggers in Jenkins with Real-Time Examples**

***

#### 🔹 1. **Poll SCM**

**Use Case:** Automatically build when code changes are pushed to Git (without a webhook).

**✅ Scenario:**

You want Jenkins to check the Git repository every 5 minutes. If changes are detected, it should trigger a build.

**🧪 Steps to Demo:**

1. Go to your Jenkins job → **Configure**
2. Scroll to **Build Triggers**
3. Check ✅ **Poll SCM**
4.  In the **Schedule** box, enter:

    ```
    H/5 * * * *
    ```

    (This means: check every 5 minutes)
5. Save the job.
6. Make a commit in your Git repo → push to the remote.
7. Jenkins will poll every 5 minutes and run a build if it detects changes.

***

#### 🔹 2. **Build Periodically**

**Use Case:** Run builds at scheduled times (like cron jobs).

**✅ Scenario:**

You want to run a job every night at 1 AM to back up logs or run a report.

**🧪 Steps to Demo:**

1. Jenkins job → **Configure**
2. Under **Build Triggers**, check ✅ **Build periodically**
3.  Add schedule:

    ```
    CopyEdit0 1 * * *
    ```

    (This means: run at 1:00 AM every day)
4. Save → Wait for the scheduled time → Jenkins runs the job automatically.

***

#### 🔹 3. **GitHub hook trigger for GITScm polling**

**Use Case:** Trigger builds instantly when changes are pushed to GitHub.

**✅ Scenario:**

As soon as a developer pushes code to GitHub, Jenkins should run the job (faster than polling).

**🧪 Steps to Demo:**

1. Jenkins job → **Configure**
2. Under **Build Triggers**, check ✅ **GitHub hook trigger for GITScm polling**
3. Save the job.
4. Go to GitHub repo → **Settings → Webhooks**
5. Add a new webhook:
   * Payload URL: `http://<your-jenkins-url>/github-webhook/`
   * Content type: `application/json`
   * Choose: **Just the push event**
6. Push a commit to GitHub → Jenkins job triggers instantly.

***

#### 🔹 4. **Trigger builds remotely (with authentication token)**

**Use Case:** Trigger Jenkins jobs from external systems (like scripts, other tools, or pipeline integrations).

**✅ Scenario:**

You want to run a job from a custom Python script using a Jenkins API call.

**🧪 Steps to Demo:**

1. Jenkins job → **Configure**
2. Check ✅ **Trigger builds remotely**
3. Enter a token (e.g., `mytoken123`)
4. Save.
5.  URL format:

    ```
    http://<jenkins-url>/job/<job-name>/build?token=mytoken123
    ```
6.  Run from browser or CURL:

    ```bash
    curl http://<jenkins-url>/job/<job-name>/build?token=mytoken123
    ```
7.  #### Add a Token to a Jenkins Job

    1. **Login to Jenkins** → Go to your Freestyle job or create a new one.
    2. Click on **“Configure”** for that job.
    3. Scroll down to **Build Triggers** section.
    4. Check ✅ **Trigger builds remotely (e.g., from scripts)**
    5.  In the **Authentication Token** field, enter a token like:

        ```
        nginxCopyEditmysecrettoken123
        ```
    6. Click **Save**.

    #### 🔗 Trigger the Build Using the Token

    Now you can trigger this job remotely using a browser or script.

    **📌 URL Format:**

    ```
    http://<jenkins-host>:<port>/job/<job-name>/build?token=<your-token>
    ```

    **📌 Example:**
8.

    If your Jenkins is running on `http://localhost:8080`, job name is `my-job`, and token is `mysecrettoken123`, then:

    ```
    http://localhost:8080/job/my-job/build?token=mysecrettoken123
    ```

    ####

```
curl -u "admin:1a2b3c4d5e6f7g" "http://44.223.61.165:8080/job/test-job/build?token=mytoken123"
```

***

####

#### 🔹 5. **Build after other projects are built**

**Use Case:** Chain jobs (Job A triggers Job B after success).

**✅ Scenario:**

You want to run a testing job only after the deployment job finishes.

**🧪 Steps to Demo:**

1. Create two jobs: **Job-A** and **Job-B**
2. Go to **Job-B** → **Configure**
3. Under **Build Triggers**, check ✅ **Build after other projects are built**
4. Enter: `Job-A`
5. Save.
6. Now, run **Job-A** → After completion, **Job-B** starts automatically.

***

#### 🔹 6. **Parameterized Trigger (Advanced using plugins)**

**Use Case:** Trigger another job and pass parameters.

**✅ Scenario:**

Job A deploys an app, and then Job B runs a test suite with environment as a parameter.

**🧪 Steps to Demo:**

1. Install **Parameterized Trigger Plugin**
2. Go to **Job-A** → Configure
3. Under **Post-build Actions**, choose:
   * ✅ **Trigger parameterized build on other projects**
4. Enter job name: `Job-B`
5. Pass parameter: `ENV=dev`
6. Save and run → Job-B is triggered with `ENV=dev`.

***

#### 🔹 7. **Scripted Trigger via Pipeline (Using `triggers {}` block in Jenkinsfile)**

**✅ Scenario:**

You want to define triggers as code inside the Jenkinsfile.

**🧪 Sample Jenkinsfile:**

```groovy
groovyCopyEditpipeline {
    triggers {
        pollSCM('H/5 * * * *')   // Equivalent to Poll SCM
    }
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }
}
```

***

### 🧑‍🏫 Teaching Tip

While teaching:

* Start with a Freestyle project
* Show a simple job first (like printing date or echo "Build triggered")
* Gradually show one trigger at a time
* Use GitHub + webhook + Poll SCM for real-world impact
* Allow students to test triggering via CURL or Git push

***

Do you want a combined Jenkins demo job with all trigger types? I can generate it for you as a ready-made teaching package.

Ask ChatGPT

