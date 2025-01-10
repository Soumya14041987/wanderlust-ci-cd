pipeline {
    agent any 
    environment{
        SONAR_HOME = tool "Sonarqube-scanner"
    }
    stages{
        stage("Clone code from GitHub"){
            steps{
                git url:"https://github.com/Soumya14041987/wanderlust-ci-cd.git", branch: "main"
            }
        }
        stage("Sonarqube QAuality Analysis"){
            steps{
               withSonarQubeEnv("Sonar"){
                   sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.ProjectName=wanderlust -Dsonar.projectKey=wanderlust -Dsonar.login=squ_8df7cd8c2691699b93c06a068c277f3ee5a210b3"
               }
            }
        }
        stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'OWASP'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("Quality Gate scan"){
            steps{
                timeout (time: 5, unit: "MINUTES"){
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy File System scan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
    }
    
}
