pipeline {
    agent any
    tools {
        maven 'apache-maven-3.6.0' // Assurez-vous que cette version est configurée dans Jenkins
    }
    environment {
        SONARQUBE_ENV = 'sonar.tools.devops.****' // Nom de l'environnement SonarQube configuré dans Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'axel', url: 'https://github.com/AxelGiff/HelloWorldMaven.git'
            }
        }
        stage('Tag Git') {
            steps {
                script {
                    sh 'git config --global user.name "AxelGiff"'
                    sh 'git config --global user.email "axel.giffard95@gmail.com"'
                    sh 'git tag -a v-${BUILD_NUMBER} -m "Jenkins Git plugin tagging with v-${BUILD_NUMBER}"'
                    sh 'git push origin v-${BUILD_NUMBER}'
                }
            }
        }

        stage('Build') {
            steps {
                withMaven(maven: 'apache-maven-3.6.0') {
                    sh "mvn clean compile"
                    sh "mvn package"
                    sh "mvn exec:java"
                }
            }
        }
        stage('Test') {
            steps {
                withMaven(maven: 'apache-maven-3.6.0') {
                    sh "mvn test"
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(SONARQUBE_ENV) {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=myProject -Dsonar.sources=./src'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy') {
            steps {
                withMaven(maven: 'apache-maven-3.6.0') {
                    sh "mvn deploy"
                }
            }
        }
    }
}
