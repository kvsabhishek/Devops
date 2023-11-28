# Jenkins pipeline syntax #
```
pipeline {
    agent any
    environment {
        CC = 'clang'
    }
    stages {
        stage('Stage') {
            steps {
                echo 'Hello world!'
                echo '${CC}' 
            }
        }
    }

    post { 
        always {
            echo 'Runs whether job is success or not'
        }

        success {
            echo 'Runs when job is success'
        }

        failure {
            echo 'Runs when job is failure'
        }
    }
}
```
# PIPE LINE DIRECTIVES #

## AGENT ##
To run on specific agent like VM(node), Docker container etc.
```
pipeline {
    agent { node { label '<agent_label>'} } 
    .
    .
}
```
We can also have agent for specific stage in the pipeline
```
pipeline {
    agent none
    stages {
        stage('Example') {
            agent any
            options {
                timeout(time: 1, unit: 'SECONDS')
            }
            steps {
                echo 'Hello World'
            }
        }
    }
}
```
## OPTIONS ##
The options directive allows configuring Pipeline-specific options from within the Pipeline itself
```
pipeline {

    options {
        ansiColor('xterm')
    }
}
```


## VARIABLES ##

-  ### Script Variable ###    
    In Jenkins, when you define variables using the def keyword, you are typically working with Groovy scripts.
    ```
    pipeline {
        agent any

        stages {
            stage('Example') {
                steps {
                    script {
                        // Define variables using def keyword
                        def stringValue = "Hello, Jenkins!"
                        def intValue = 42
                        def floatValue = 3.14
                        def booleanValue = true

                        // You can also define lists and maps
                        def listValue = [1, 2, 3]
                        def mapValue = [key1: 'value1', key2: 'value2']

                        // Print the values
                        echo "String: ${stringValue}"
                        echo "Integer: ${intValue}"
                        echo "Float: ${floatValue}"
                        echo "Boolean: ${booleanValue}"
                        echo "List: ${listValue}"
                        echo "Map: ${mapValue}"
                    }
                }
            }
        }
    }

    ```
-  ### Environment Variables ###
    The environment directive specifies a sequence of key-value pairs which will be defined as environment variables for all steps, or stage-specific steps, depending on where the environment directive is located within the Pipeline.
    ```
    pipeline {
        agent any
        environment {
            CC = 'clang'
        }
        stages {
            stage('Example') {
                environment {
                    AN_ACCESS_KEY = credentials('my-predefined-secret-text')
                }
                steps {
                    sh 'printenv'
                }
            }
        }
    }
    ``` 
    - To save credentials as ENV variables.

        __credentials()__ --> function that can get saved passwords in Jenkins.
        when credentials function is assigned to a variable(VAR). USER NANME is stored in a variable called __VAR_USR__ and password is stored in a variable __VAR_PSW__ 
        ```
        pipeline {
            agent any
            stages {
                stage('Example Username/Password') {
                    environment {
                        SERVICE_CREDS = credentials('my-predefined-username-password')
                    }
                    steps {
                        sh 'echo "Service user is $SERVICE_CREDS_USR"'
                        sh 'echo "Service password is $SERVICE_CREDS_PSW"'
                        sh 'curl -u $SERVICE_CREDS https://myservice.example.com'
                    }
                }
                stage('Example SSH Username with private key') {
                    environment {
                        SSH_CREDS = credentials('my-predefined-ssh-creds')
                    }
                    steps {
                        sh 'echo "SSH private key is located at $SSH_CREDS"'
                        sh 'echo "SSH user is $SSH_CREDS_USR"'
                        sh 'echo "SSH passphrase is $SSH_CREDS_PSW"'
                    }
                }
            }
        }
        ```
* ### Parameters ###
The parameters directive provides a list of parameters that a user should provide when triggering the Pipeline. The values for these user-specified parameters are made available to Pipeline steps via the __params object__.
```
pipeline {
    agent any
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
        stage('Example') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
        }
    }
}
```

## TRIGGERS ##
The triggers directive defines the automated ways in which the Pipeline should be re-triggered.
### Supported triggers are : ###
* __cron__ --> Specifies time/interval for triggering the pipeline
* __pollSCM__ --> Specifies the interval to check the repo for any new changes. If changes available pipeline will be triggered.
* __upstream__ --> Accepts a comma-separated string of jobs and a threshold. When any job in the string finishes with the minimum threshold, the Pipeline will be re-triggered. For example: triggers { upstream(upstreamProjects: 'job1,job2', threshold: hudson.model.Result.SUCCESS) }
```
pipeline {
    agent any
    triggers {
        cron('H */4 * * 1-5')
    }
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```

## INPUT ##
The input directive on a stage allows you to prompt for input, using the input step. The stage will pause after any options have been applied, and before entering the agent block for that stage or evaluating the when condition of the stage. If the input is approved, the stage will then continue. Any parameters provided as part of the input submission will be available in the environment for the rest of the stage.
    
* __message__ --> to display at the time of providing input.

* __ok__ --> Optiona text for ok button.

* __submitter__ --> An optional comma-separated list of users or external group names who are allowed to submit this input.

* __parameter__ -- > An optional list of parameters to prompt the submitter to provide.

```
pipeline {
    agent any
    stages {
        stage('Example') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
    }
}
```
```
pipeline {
    agent any 
    stages {
        stage('APPROVE') {
            steps {
                 input "Shall I apply?"
            }
        }

        stage('APPLY') {
            steps {
                echo 'Hello world! From stage 2'
                sh 'ls -ltr'
            }
        }
    }
}
```
## STEPS ##

* echo --> To print the content
```
stage('Stage 1') {
    steps {
        echo 'Hello world!' 
    }
}
```
* sh --> To run any linux command
```
steps {
    sh 'ls -ltr'
    sh '''
        ls -ltr
        pwd
        echo "Hello"
    '''
}
```

## WHEN ##
The when directive allows the Pipeline to determine whether the stage should be executed depending on the given condition.

### Comparators that are supported are: ###
The optional parameter comparator may be added after an attribute to specify how any patterns are evaluated for a match:

* __EQUALS__ for a simple string comparison 

* __GLOB__ (the default) for an ANT style path glob

* __REGEXP__ for regular expression matching. EX : when { changeset pattern: ".TEST\\.java", comparator: "REGEXP" }

### Some of the supported built in conditions: ###

* __branch__ --> To check the branch on which pipeline is executing.
```
when { branch 'master' }
```
* __buildingTag__ -->
```
when { buildingTag() }
```
* __changelog__ -->  Execute the stage if the build’s SCM changelog contains a given regular expression pattern.
```
when { changelog '.*^\\[DEPENDENCY\\] .+$' }.
```
* __ changeset__ --> Execute the stage if the build’s SCM changeset contains one or more files matching the given pattern.
```
when { changeset pattern: ".TEST\\.java", comparator: "REGEXP" }
```
* __changeRequest__ --> Executes the stage if the current build is for a "change request" (a.k.a. Pull Request)
```
when { changeRequest target: 'master' }
```
* __environment__ --> Execute the stage when the specified environment variable is set to the given value.
```
when { environment name: 'DEPLOY_TO', value: 'production' }
```
* __equals__ --> Execute the stage when the expected value is equal to the actual value. 
```
when { equals expected: 2, actual: currentBuild.number }
```
* __expression__ --> Execute the stage when the specified Groovy expression evaluates to true.
```
when { expression { return params.DEBUG_BUILD } }
```
* __tag__ --> Execute the stage if the TAG_NAME variable matches the given pattern.
```
when { tag pattern: "release-\\d+", comparator: "REGEXP"}
```
* __not__ --> Execute the stage when the nested condition is false.
```
when { not { branch 'master' } }
```
* __allOf__ --> Execute the stage when __all of the nested conditions__ are true. Must contain at least one condition.
```
when { allOf { 
            branch 'master'; 
            environment name: 'DEPLOY_TO', value: 'production' 
            } 
    }
```
* __anyOf__ --> execute the stage when __at least one of the nested conditions__ is true
```
when { anyOf { 
            branch 'master'; 
            environment name: 'DEPLOY_TO', value: 'production' 
            } 
    }
```
* __beforeAgent__ -->  f beforeAgent is set to true, the when condition will be evaluated first, and the agent will only be entered if the when condition evaluates to true.
* __beforeInput__ --> If beforeInput is set to true, the when condition will be evaluated first, and the input will only be entered if the when condition evaluates to true.
* __beforeOptions__ --> If beforeOptions is set to true, the when condition will be evaluated first, and the options will only be entered if the when condition evaluates to true.
```
pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                branch 'production'
                anyOf {
                    environment name: 'DEPLOY_TO', value: 'production'
                    environment name: 'DEPLOY_TO', value: 'staging'
                }
            }
            steps {
                echo 'Deploying'
            }
        }
    }
}
```

## PARALLEL ##
By default all the stages inside the pipeline will run in sequential order. If we want them to run parallely without depending on one another we use parallel directive.
```
pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        parallel {
            stage('In Parallel 1') {
                steps {
                    echo "In Parallel 1"
                }
            }
            stage('In Parallel 2') {
                steps {
                    echo "In Parallel 2"
                }
            }
        }
    }
}
```

# UP STREAM & DOWN STRAM JOBS #
If we want to trigger another pipeline if the triggered pipeline is success then we use this concept.
JOB-A --> Triggered Job
JOB-B --> If JOB-A is success then JOB-B should trigger. </br>

__JOB-A is considered as the UP Stream job from JOB-B and JOB-B is considered as the downstream job for JOB-A__
### UP STREAM ###
```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the Upstream Job'
                script {
                    def params = [
                        string(name: 'version', value: "1.0.2")
                    ]
                    writeFile(file: 'example.txt', text: 'This is an example file')
                }
            }
        }
    }

    post {
        success {
            build job: '../downstream-job', propagate: true, wait: true, parameters: params //To call the dwonsteam job
        }
    }
}
```
### DOWN STREAM ##
```
pipeline {
    agent any
    parameters{
        string(name: 'version', defaultValue:'1.0.0', description: 'artifact version')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the Downstream Job'
                script {
                    def fileContent = readFile('../upstream-job/example.txt')
                    echo "Content of the upstream file: ${fileContent}"
                    echo "Artifact version: ${parms.version}"
                }
            }
        }
    }
}
```
# SONARQUBE #
Sonarqube is used for static source code analysis.

Scanning Types:
* Static source code analysis --> Checks the source code, analyse it and give recomendations.(__Sonarqube)__
* Static application security testing --> Checks the code based on security perspective.(__Fortify__)
* Dynamic application security testing --> Mocks the process of the code at runtime, mocks the process of hacking.
* Open source library scanning --> Libraries you are getting from internet are scanned.(__Nexus IQ__)
* Image Scan --> Scan docker images.(__Twistlock__)
```
 pipeline {
    agent any
    stages {
        stage('Sonar Scan') {
            steps {
                sh 'sonar-scanner'
            }
        }
    }
 }
```

# NEXUS #
Used to store artifacts.
To push artifacts to nexus need to install plugin __Nexus Artifactory Uploader__.
```
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'zip -r catalouge.zip ./* --exclude=.git --exclude=.zip'
            }
        }

        stage('Publish Artifact to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '52.71.253.240:8081/',
                    groupId: 'com.roboshop',
                    version: 1.0.0,
                    repository: 'Catalogue',
                    credentialsId: 'CredentialsId',
                    artifacts: [
                        [artifactId: Catalogue,
                        classifier: '',
                        file: 'catalogue-' + version + '.zip',
                        type: 'zip']
                    ]
                )
            }
        }
    }

    post{
        always{
            echo 'Cleaning Workspace'
            deleteDir() //Once deployment is done it is mandate to clean the wrokspace directory as a best practice.
        }
    }
}
```
Developers use package.json file where they will mention the version of the code. In above example artifact version in Fixed. To make it dynamic we need to read the version from package.json and use it.

To read json file we need to install a plugin __Pipeline Utility Steps__
### To read a value from JSON file inside pipeline ###
```
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'zip -r catalouge.zip ./* --exclude=.git --exclude=.zip'
            }
        }

        stage('Publish Artifact to Nexus') {
            steps {
                script {
                    def packageJson = readJSON(file: 'package.json')
                    def packageVersion = packageJson.version

                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: '52.71.253.240:8081/',
                        groupId: 'com.roboshop',
                        version: ${packageVersion},
                        repository: 'Catalogue',
                        credentialsId: 'CredentialsId',
                        artifacts: [
                            [artifactId: Catalogue,
                            classifier: '',
                            file: 'catalogue-' + version + '.zip',
                            type: 'zip']
                        ]
                    )
                }
            }
        }
    }

    post{
        always{
            echo 'Cleaning Workspace'
            deleteDir() //Once deployment is done it is mandate to clean the wrokspace directory as a best practice.
        }
    }
}
```