# Integrating SonarQube with Jenkins

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*a54YW3r0MHy6E1qlR2IMmQ.png" alt="" height="347" width="700"><figcaption></figcaption></figure>

### Introduction: <a href="#id-96bd" id="id-96bd"></a>

In modern software development workflows, ensuring code quality is paramount. SonarQube and Jenkins are two powerful tools that, when integrated, can help achieve this goal efficiently. In this guide, we’ll walk through the process of setting up SonarQube using Docker, configuring it in Jenkins UI, and integrating it into a Jenkins pipeline job to scan code before building.

### Prerequisites: <a href="#d0c7" id="d0c7"></a>

1. Installed Docker
2. Installed Jenkins
3. Basic familiarity with Docker and Jenkins

### Step 1: Setting Up SonarQube <a href="#id-8c54" id="id-8c54"></a>

Using Docker First, let’s pull the SonarQube Docker image and run it.

```
docker pull sonarqube
docker run -d --name sonarqube -p 9000:9000 sonarqube
```

Access SonarQube UI by navigating to [http://your-IP-address:9000](http://localhost:9000/) in your web browser. Follow the setup wizard to create an admin account.

Default login credentials are admin/admin.

### Step 2: Generate SonarQube Token <a href="#id-30b7" id="id-30b7"></a>

2.1. Log in to SonarQube using the admin credentials.

2.2. Navigate to “My Account” -> “Security” -> “Generate Tokens”.

2.3. Provide a name for the token and click on “Generate”. Save the generated token securely.

**To set up SonarQube credentials in Jenkins:**

1. Go to “Manage Jenkins” > “Manage Credentials”.
2. Choose the appropriate domain.
3. Click “Add Credentials”.
4. Select the credential type (e.g., “Secret text” for tokens, “Username with password” for username/password).
5. Enter the credential details.
6. Provide an ID and description.
7. Save the credentials.

### Step 3: Create a New Project: <a href="#df9b" id="df9b"></a>

Once logged in, you need to create a new project in SonarQube. Follow these steps:

* Click on the “Projects” tab on the top menu.
* Click on the “Create Project” button.
* Provide a unique project key and a display name for your project.
* Choose the appropriate language for your project. SonarQube supports various programming languages.
* Click on “Set Up” to proceed.

**Configure Analysis Settings**: After setting up the project, you’ll be prompted to configure the analysis settings. This involves configuring how SonarQube will analyze your code.

### Step 4: Configuring SonarQube Properties: <a href="#id-0ab1" id="id-0ab1"></a>

It can offer benefits in terms of organization, consistency, and ease of maintenance, especially for projects with specific configuration needs

* Create a `sonar-project.properties` file in your project directory.
* Add necessary properties, e.g.,

```
sonar.projectKey=lil_sonar_project
```

### Step 5: Installing SonarQube Scanner in Jenkins <a href="#ad33" id="ad33"></a>

* Open Jenkins dashboard and navigate to `Manage Jenkins` > `Global Tool Configuration`.
* Scroll down to the `SonarQube Scanner` section.
* Click on `Add SonarQube Scanner` and provide a name for the installation.
* Specify the path to the SonarQube scanner installation directory.
* Save the configuration.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*qabecfcms-k4z4ZSwF7IqA.png" alt="" height="456" width="700"><figcaption></figcaption></figure>

### Step 6: Install SonarQube Plugin <a href="#e857" id="e857"></a>

To run your project analyses with Jenkins, the following plugins must be installed and configured:

* SonarQube Scanner plugin for Jenkins — version 2.11 or later

Navigate to Jenkins Dashboard -> “Manage Jenkins” -> “Manage Plugins”.

Go to the “Available” tab, search for “SonarQube Scanner” plugin, and install it.

### Step 7: Configure SonarQube in Jenkins <a href="#ed6d" id="ed6d"></a>

* Navigate to Jenkins Dashboard -> “Manage Jenkins” -> “Configure System”.
* Scroll down to the “SonarQube servers” section and click on “Add SonarQube”.
* Provide a name for the SonarQube server.
* Enter the SonarQube server URL (http://IP\_SonarQube:9000).
* Paste the SonarQube token generated in Step 2.
* Save the configuration.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*BVN5mqbEEDloLOC3WUeVzw.png" alt="" height="352" width="700"><figcaption></figcaption></figure>

### Step 8: Integrate SonarQube into Jenkins Pipeline <a href="#id-4585" id="id-4585"></a>

Open or create a Jenkinsfile in your project.

Define the SonarQube scanner configuration within the pipeline script:

```
pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps{
           git branch: 'main', url: 'https://github.com/lily4499/lil-node-app.git'
            }
        }
        // run sonarqube test
        stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'lil-sonar-tool';
            }
            steps {
              withSonarQubeEnv(credentialsId: 'lil-sonar-credentials', installationName: 'lil sonar installation') {
                sh "${scannerHome}/bin/sonar-scanner"
              }
            }
        }
    }
}
```

Make sure to replace SonarQube Tool, Installation and Credentials with the names you gave to your configurations in Jenkins.

### Step 9: Checking SonarQube Configuration: <a href="#id-13dc" id="id-13dc"></a>

* Run the Jenkins pipeline job.
* Once the SonarQube stage executes, check the SonarQube dashboard (http://localhost:9000) for the analysis report.
* Verify that the code quality metrics are populated correctly.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*N9pUPFOmFWEC5YO7CXkD8Q.png" alt="" height="361" width="700"><figcaption></figcaption></figure>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*VPqmbg27BZOCtpI5OG8pBw.png" alt="" height="349" width="700"><figcaption></figcaption></figure>

### &#x20;<a href="#id-38dd" id="id-38dd"></a>

\
