pipeline {
    agent any
        tools {
             // Utiliza el nombre configurado para Maven en tu instancia de Jenkins.
             maven 'Maven-3.9.4'
             // Utiliza el nombre configurado para JDK en tu instancia de Jenkins.
             jdk 'Java-11.0.19'
         }

    stages {
        stage('Checkout') {
            steps {
                // Clonar el repositorio de tu proyecto desde Git
                checkout([$class: 'GitSCM', branches: [[name: '2.1.x']],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [[$class: 'CloneOption', noTags: false, reference: '', shallow: false, timeout: 10]],
                          submoduleCfg: [],
                          userRemoteConfigs: [[url: ' https://github.com/alexisdona/spring-petclinic-migration.git']]])
            }
        }

        stage('Clean install') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Run Test') {
                    steps {
                        sh 'mvn test'
                    }
        }

          stage('Run given recipes') {
                    steps {
                                sh 'mvn rewrite:run -Drewrite.activeRecipes=org.openrewrite.java.format.AutoFormat,org.openrewrite.java.RemoveUnusedImports'
                            }
                }

          stage("Create artifacts or make changes") {
                    steps {
                                sh "touch testfile"
                                sh "git add ."
                                sh "git commit -m 'Add testfile from Jenkins Pipeline'"
                            }
               }

          stage("Push to Git Repository") {
                    steps {
                              withCredentials([gitUsernamePassword(credentialsId: 'a33f5f1b-1b80-47f8-bee9-5061fe836c9c', gitToolName: 'Default')]) {
                                            sh "git push -u origin 2.1.x"
                                        }
                                    }

                    }
            }
                            post {
                                always {
                                    deleteDir()
                                }
                            }

}