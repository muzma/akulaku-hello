pipeline {
    agent { label 'win'}

    stages {
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/-'
               
                 bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {

                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('Build Docker') {
            steps {
                bat "docker build -t muzma:v1 ."
                bat "docker tag muzma:v1 muzma/spring-docker-akulaku:v1"
                bat "docker push muzma/spring-docker-akulaku:v1"
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                    bat "kubectl apply -f deploy.yaml"
                    bat "kubectl apply -f services.yaml"
                }
            }
        stage('Curl') {
            steps {
                    bat "curl localhost:30009"
                }
            }
    }
}