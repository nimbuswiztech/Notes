# Pipelines

#### ✅ **any — free agent/slave**

```groovy
pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'mkdir abc'
                sh 'ls -lart'
                sh 'pwd'
            }
        }
    }
}
```

***

#### ✅ **none — schedule stages on different agents**

```groovy
pipeline {
    agent none
    stages {
        stage('Hello') {
            agent any
            steps {
                echo 'Hello World'
                // sh 'mkdir abc'
                sh 'ls -lart'
                sh 'pwd'
            }
        }
        stage('Hello1') {
            agent any
            steps {
                echo 'Hello World'
                // sh 'mkdir abc'
                sh 'ls -lart'
                sh 'pwd'
            }
        }
    }
}
```

***

#### ✅ **label — specific agent**

```groovy
pipeline {
    agent {
        label 'salve01'
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'mkdir abc'
                sh 'ls -lart'
                sh 'pwd'
            }
        }
    }
}
```

***

#### ✅ **docker — run inside container**

```groovy
pipeline {
    agent {
        docker {
            image 'ubuntu:latest'
        }
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'mkdir abc'
                sh 'ls -lart'
                sh 'pwd'
            }
        }
    }
}
```

***

#### ✅ **pipeline with environment block**

```groovy
pipeline {
    agent any
    environment {
        test = "xy"
    }
    stages {
        stage('Hello') {
            steps {
                cleanWs()
                echo 'Hello World'
                sh 'mkdir $folder'
                sh 'ls -lart'
                sh 'pwd'
                print BUILD_URL
                sh "echo tool name is $tool"
            }
        }
    }
}
```

***

#### ✅ **pipeline with options (retry, buildDiscarder, timeout, timestamps)**

```groovy
pipeline {
    agent any
    environment {
        test = "xy"
    }
    parameters {
        string(name: 'pet', defaultValue: 'jimmy', description: 'name of pet')
    }
    options {
        // retry(3)
        // buildDiscarder(logRotator(numToKeepStr: '10', daysToKeepStr: '30'))
        // disableConcurrentBuilds()
        timeout(time: 20, unit: 'SECONDS')
        timestamps()
    }
    stages {
        stage('test') {
            steps {
                cleanWs()
                sh """
                echo "helloWorld" > a
                echo "helloWorld" > b
                echo "helloWorld" > c
                echo "helloWorld" > d
                """
                sh "ls -lart && pwd"
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'ls -lart'
                sh 'pwd'
                print BUILD_URL
                sh "echo tool name is $tool"
                print pet
                retry(3) {
                    sh "ls -lart"
                    sleep 60
                }
            }
        }
    }
}
```

***

#### ✅ **parallel stages with try/catch and `dir`**

```groovy
pipeline {
    agent any
    options {
        timestamps()
    }
    stages {
        stage('parallel') {
            parallel {
                stage('projectA') {
                    steps {
                        cleanWs()
                        sh "echo projectA is running 1st time"
                        sh "echo projectA is running 2st time"
                        sh "echo projectA is running 3st time"
                    }
                }
                stage('projectB') {
                    steps {
                        sh "echo projectB is running 1st time"
                        sh "echo projectB is running 2st time"
                        sh "echo projectB is running 3st time"
                    }
                }
            }
        }
        stage('test') {
            steps {
                script {
                    sh "mkdir testfolder"
                    dir('testfolder') {
                        try {
                            sh 'mkdir devopstest'
                            sh "echo hello > a"
                            sh 'cat a'
                        } catch (Exception e) {
                            echo 'folder already exist'
                        }
                        sh 'ls -lart && pwd'
                    }
                }
            }
        }
    }
}
```

***

#### ✅ **trigger other jobs using `build()`**

```groovy
pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('test') {
            steps {
                cleanWs()
                sh """
                echo "helloWorld" > a
                echo "helloWorld" > b
                echo "helloWorld" > c
                echo "helloWorld" > d
                """
                sh "ls -lart && pwd"
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'ls -lart'
                sh 'pwd'
                print BUILD_URL
                retry(3) {
                    sh "ls -lart"
                }
                build('projectA/test')
                build('projectB/test')
            }
        }
    }
}
```

***

#### ✅ **post block - always, success, failure, etc.**

```groovy
pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('test') {
            steps {
                cleanWs()
                sh """
                echo "helloWorld" > a
                echo "helloWorld" > b
                echo "helloWorld" > c
                echo "helloWorld" > d
                """
                sh "ls -lart && pwd"
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'ls -lart'
                sh 'pwd'
                print BUILD_URL
                sh "ls -lartiou"
            }
        }
    }
    post {
        always {
            echo 'verify always post action'
        }
        success {
            build('projectA/test')
        }
        failure {
            cleanWs()
        }
    }
}
```

***

#### ✅ **post inside stage**

```groovy
pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('test') {
            steps {
                cleanWs()
                sh """
                echo "helloWorld" > a
                echo "helloWorld" > b
                echo "helloWorld" > c
                echo "helloWorld" > d
                """
                sh "ls -lart && pwd"
            }
            post {
                always {
                    echo 'verify always post action'
                }
                success {
                    build('projectA/test')
                }
                failure {
                    cleanWs()
                }
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'ls -lart'
                sh 'pwd'
                print BUILD_URL
                sh "ls -lartiou"
            }
        }
    }
}
```

***

#### ✅ **when condition with expression**

```groovy
pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('test') {
            when {
                allOf {
                    expression { env_nonprod == 'uat' }
                    expression { env_non == 'both' }
                }
            }
            steps {
                cleanWs()
                sh """
                echo "helloWorld" > a
                echo "helloWorld" > b
                echo "helloWorld" > c
                echo "helloWorld" > d
                """
                sh "ls -lart && pwd"
                echo 'uat options'
            }
        }
        stage('Hello') {
            when {
                allOf {
                    expression { env_nonprod == 'qa' }
                    expression { env_non == 'both' }
                }
            }
            steps {
                echo 'Hello World'
                sh 'ls -lart'
                sh 'pwd'
                print BUILD_URL
                echo 'qa options'
            }
        }
    }
}
```

***

#### ✅ **when condition with parameters**

```groovy
pipeline {
    agent any
    parameters {
        string(name: 'env_nonprod', defaultValue: 'uat', description: 'Env')
        string(name: 'env_non', defaultValue: 'both', description: 'Target')
    }
    stages {
        stage('test') {
            when {
                allOf {
                    expression { params.env_nonprod == 'uat' }
                    expression { params.env_non == 'both' }
                }
            }
            steps {
                cleanWs()
                sh """
                echo "helloWorld" > a
                echo "helloWorld" > b
                echo "helloWorld" > c
                echo "helloWorld" > d
                """
                sh "ls -lart && pwd"
                echo 'uat options'
                print cred
            }
        }
        stage('Hello') {
            when {
                allOf {
                    expression { params.env_nonprod == 'qa' }
                    expression { params.env_non == 'both' }
                }
            }
            steps {
                echo 'Hello World'
                sh 'ls -lart'
                sh 'pwd'
                print BUILD_URL
                echo 'qa options'
            }
        }
    }
}
```

***

#### ✅ **Git + Maven build**

```groovy
pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                git branch: 'main', credentialsId: 'siddeshPAT', url: 'https://github.com/Siddeshg672/hello_world_public_war.git'
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}
```
