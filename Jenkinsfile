pipeline {
    agent any
    environment {
        SONAR_HOME = tool "Sonar"
    }
    stages {
        stage("Code Checkout"){ 
        steps{
            git(
                url: "https://github.com/adissonaires/Capstone-Project.git",
                branch: "main",
                credentialsId: "jenkins-secret",
                changelog: true,
                poll: true
            )
        }
    }
        stage("SonarQube Code Analysis") {
        steps {
            withSonarQubeEnv("Sonar") {
                sh """
                        $SONAR_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=airbyte \
                        -Dsonar.projectKey=airbyte \
                        -Dsonar.exclusions=**/*.java
                    """
            }
        }
    }
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs --format  table -o trivy-fs-report.html ."
            }
        }
        stage("OWASP Dependency Check"){
            steps{
                echo "Skipping due to some dependency test cases are yet to be merged to master"
                // dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
                // dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("PROD Deployment"){
            steps{
              sh "wget https://raw.githubusercontent.com/airbytehq/airbyte/master/run-ab-platform.sh && chmod +x run-ab-platform.sh"
              echo "Test Cases check" 
              sh "./run-ab-platform.sh -b"
              echo "PROD Deployment successfull" 
            }
         }
      }
   }

