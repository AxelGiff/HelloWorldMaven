pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'apache-maven-3.6.0'
        GIT_CREDENTIALS = 'a4fbac54-ac6e-4a96-9c40-ee8677c29110'  // Remplace par ton ID de credential si nécessaire
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm  // Récupère le code depuis Git
                }
            }
        }

        stage('Install Maven') {
            steps {
                script {
                    // Installation de Maven sur Jenkins
                    tool name: 'apache-maven-3.6.0', type: 'Maven'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Compilation du projet Maven
                    sh "'${MAVEN_HOME}/bin/mvn' clean install"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Exécution des tests unitaires
                    sh "'${MAVEN_HOME}/bin/mvn' test"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Effectuer l'analyse avec SonarQube
                    withSonarQubeEnv('SonarQube') { 
                        sh "'${MAVEN_HOME}/bin/mvn' sonar:sonar"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Vérification de l'état de la qualité après l'analyse SonarQube
                    waitForQualityGate()
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Déploiement (exemple d'un déploiement simple)
                    sh 'echo "Déploiement en cours..."'  // Remplacer par ton script de déploiement
                }
            }
        }
    }

    post {
        always {
            // Actions à effectuer après l'exécution du pipeline, peu importe le résultat
            cleanWs()  // Nettoyage de l'espace de travail
        }
        success {
            // Actions en cas de succès
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            // Actions en cas d'échec
            echo 'Le pipeline a échoué.'
        }
    }
}
