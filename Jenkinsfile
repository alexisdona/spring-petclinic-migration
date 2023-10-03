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

          stage('Save recipes execution') {
                           steps {
                                           script {
                                               // Configura las credenciales de Git si es necesario
                                               withCredentials([usernamePassword(credentialsId: 'git-credentials', usernameVariable: 'alexisdona', passwordVariable: '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')]) {
                                                   // Realiza los cambios en el repositorio
                                                   sh '''
                                                       git config user.email "jenkins@example.com"
                                                       git config user.name "Jenkins automatic commit "
                                                       git add .
                                                       git commit -m "Ejecución de receta"
                                                       git push origin 2.1.x
                                                   '''

                                           }
                }
    }

  /*   post {
        success {
            // Este bloque se ejecutará si la etapa anterior fue exitosa
            // Puedes agregar notificaciones o tareas adicionales aquí
        }

        failure {
            // Este bloque se ejecutará si la etapa anterior falló
            // Puedes agregar acciones de manejo de errores aquí
        }
    } */
}
