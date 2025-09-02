# Matrix-based Security

###

Matrix-based security allows Jenkins administrators to assign **specific permissions per user or group** using a matrix layout. Each row is a user or group, and each column is a permission like:

| User/Group | Job                          | Overall      | Agent       | Credentials |
| ---------- | ---------------------------- | ------------ | ----------- | ----------- |
| admin      | ✅ Build ✅ Configure ✅ Delete | ✅ Administer | ✅ Configure | ✅ Create    |
| developer  | ✅ Build ✅ Read               | ❌            | ❌           | ❌           |
| viewer     | ✅ Read                       | ❌            | ❌           | ❌           |

***

### ✅ Step-by-Step Demo Setup

#### 🔒 1. Enable Jenkins Security

* Go to: **Manage Jenkins → Configure Global Security**
*   Under **Access Control → Authorization**, select:

    ✅ **Matrix-based security**

***

#### 👤 2. Add Users (or Groups)

Under the matrix:

* Click **"Add user or group"**
* Add the following:
  * `admin`
  * `developer`
  * `viewer`

***

#### 🧱 3. Assign Permissions

**demo permission setup**:

| User      | Overall | Job Read | Job Build | Job Configure | Job Delete | Credentials |
| --------- | ------- | -------- | --------- | ------------- | ---------- | ----------- |
| admin     | ✅ All   | ✅        | ✅         | ✅             | ✅          | ✅           |
| developer | ❌       | ✅        | ✅         | ✅             | ❌          | ❌           |
| viewer    | ❌       | ✅        | ❌         | ❌             | ❌          | ❌           |

> 🧠 Explain to students: each permission column gives access to specific Jenkins functions.

***

### 🔬 Real-Time Testing Scenario

#### 1. Log in as `admin`

* Full access to everything.
* Can create, configure, delete jobs.
* Can access system configuration.

#### 2. Log in as `developer`

* Can view and build jobs.
* Cannot delete or create new credentials.

#### 3. Log in as `viewer`

* Can only view jobs and build history.
* Cannot build or configure.

***

### ✅ Jenkinsfile for Testing Access

To test permission visibility, create a sample pipeline job:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building app..."
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying to dev environment..."
            }
        }
    }
}
```

### Per-Project Security

1. Go to **Manage Jenkins → Configure Global Security**
2. Check **"Enable project-based security"**
3. Inside each job → **Configure → Enable project-based security**
