def produccion = [:]
            produccion.name = 'cep'
            produccion.host = '172.17.0.3'
            produccion.user = 'root'
            produccion.password = 'root'
            produccion.allowAnyHosts = true
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {

                //Creo el directorio del código. Si no existe se crea

                dir('codigo') {
                    // Me descargo el fuente
                    git 'https://github.com/srodenas/hola-maven.git'

                    // Ejecuta el package saltandose los test. Generará el .war si todo es correcto.
                    sh "mvn clean package -DskipTests"

                    // To run Maven on a Windows agent, use
                    // bat "mvn -Dmaven.test.failure.ignore=true clean package"
                }

            }

            post {
                // Si el build es correcto, copia el .war dentro de la carpeta webapp
                // del jenkins para estar disponibles en las siguientes fases.
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    archiveArtifacts 'codigo/webapp/target/*.war'
                }
            }
        }
        stage('Test') {
            steps {
                sh "echo 'Realizacióyuin de algunos Test'"
            }

        }
        stage('Container') {
            steps {
                sh "echo 'Creo el contenedor'"
                dir('contenedor') {
                    withCredentials([usernamePassword(credentialsId: 'gitpruebasanti', passwordVariable: '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', usernameVariable: 'srodenas')]) {

                        git 'https://github.com/srodenas/pruebaDespliegue.git'
                        sh('''
                            cp ../codigo/webapp/target/webapp.war .
                            git add .
                            git config --global user.email 'srodher115@g.educaand.es'
                            git config --global user.name  'srodenas'
                            git commit -m 'Desde Jenkinsfile'
                            git config --local credential.helper "!f() { echo username=\\$usuario; echo password=\\$password; }; f"
                            git push origin master
                        ''')
                    }
                }

            }
        }
        stage('Deploy') {
            steps {
                sh "echo 'Desplegando en servidor remoto. Utilizo pluggins ssh-step'"
                sshPut remote: produccion, from: 'codigo/webapp/target/webapp.war' , into: '/usr/local/tomcat/webapps/'
            }

        }

    }
}
