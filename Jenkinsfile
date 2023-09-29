pipeline {
    agent any
        tools {
             // Utiliza el nombre configurado para Maven en tu instancia de Jenkins.
             maven 'Maven-3.9.4'
             // Utiliza el nombre configurado para JDK en tu instancia de Jenkins.
             jdk 'Java-8.0.372'
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

        stage('Build and Test') {
            steps {
                // Configurar el entorno de Maven
                sh 'mvn clean install' // Esto compilará y ejecutará los tests

                // Opcional: Puedes agregar otras tareas relacionadas con pruebas aquí
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
