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
                          sh "git checkout 2.1.x" //To get a local branch tracking remote
                          sh "git pull origin 2.1.x"
            }
        }

        stage('Clean install') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Run Tests') {
                    steps {
                        sh 'mvn test'
                    }
        }

          stage('Run given recipes and push to remote') {
                    steps {
                                   withCredentials([gitUsernamePassword(credentialsId: 'a33f5f1b-1b80-47f8-bee9-5061fe836c9c', gitToolName: 'Default')]) {
                                   sh 'mvn rewrite:run -Drewrite.activeRecipes=org.openrewrite.java.format.AutoFormat,org.openrewrite.java.RemoveUnusedImports'
                                   sh "git add ."
                                   sh "git commit -m 'push openrewrite changes to remote'"
                                   sh "git push -u origin 2.1.x"
                                            }
                            }
                }

        /*  stage("Push to Git Repository") {
                    steps {

                                    }

                    }
            }*/
    }
}

