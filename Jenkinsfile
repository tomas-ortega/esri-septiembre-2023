pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '7', artifactNumToKeepStr: '100'))
        timestamps()
    }

    parameters {
        choice(name: 'PARAMETERS_IN_BASE64', choices: ['No', 'Yes'], description: 'Indica si los parámetros se reciben con codificación en Base64.')
        string(name: 'NUMBER_1', defaultValue: '1', description: 'Este sería el dato del primer número entero.')
        string(name: 'NUMBER_2', defaultValue: '1', description: 'Este sería el dato del segundo número entero.')
    }

    stages {
        stage('Clean WorkSpace') {
            steps {
                cleanWs()
            }
        }

        
        stage('DEBUG: Show raw value') {
            steps {
                println "NUMBER_1: " + "$NUMBER_1"
                println "NUMBER_2: " + "$NUMBER_2"
            }
        }

        stage('Sync Git Repo') {
            steps {
                script {
                    checkout([$class: 'GitSCM',
                        branches: [[name: '**']],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [[$class: 'SubmoduleOption',
                                        recursiveSubmodules: true
                                        ]], 
                        submoduleCfg: [],
                        userRemoteConfigs: [[url: 'https://github.com/tomas-ortega/esri-septiembre-2023.git']]])
                }
            }
        }

        stage('Deployment Nexus Maven') {
            steps {
                script {
                    def currentFolderProjectName = "${WORKSPACE}".substring(28)
                    sh 'docker run --rm -v "/var/lib/docker/volumes/esri-jenkins/_data/workspace/"' + currentFolderProjectName + ':/usr/src/mymaven -w /usr/src/mymaven esri/maven-tool:3.8.6-openjdk-11 mvn install deploy'
                }
            }
        }


        stage('Verificar Maven') {
            steps {
                script {
                    def currentFolderProjectName = "${WORKSPACE}".substring(28)
                    sh 'docker run --rm -v "/var/lib/docker/volumes/esri-jenkins/_data/workspace/"' + currentFolderProjectName + ':/usr/src/mymaven -w /usr/src/mymaven esri/maven-tool:3.8.6-openjdk-11 mvn verify'
                }
            }
        }

        stage('Test Maven') {
            steps {
                script {
                    def currentFolderProjectName = "${WORKSPACE}".substring(28)
                    sh 'docker run --rm -v "/var/lib/docker/volumes/esri-jenkins/_data/workspace/"' + currentFolderProjectName + ':/usr/src/mymaven -w /usr/src/mymaven esri/maven-tool:3.8.6-openjdk-11 mvn test'
                }
            }
        }

        stage('Sonar Maven') {
            steps {
                script {
                    def currentFolderProjectName = "${WORKSPACE}".substring(28)
                    sh 'docker run --rm -v "/var/lib/docker/volumes/esri-jenkins/_data/workspace/"' + currentFolderProjectName + ':/usr/src/mymaven -w /usr/src/mymaven esri/maven-tool:3.8.6-openjdk-11 mvn sonar:sonar'
                }
            }
        }


        stage('Build artifactori Maven') {
            steps {
                script {
                    def currentFolderProjectName = "${WORKSPACE}".substring(28)
                    sh 'docker run --rm -v "/var/lib/docker/volumes/esri-jenkins/_data/workspace/"' + currentFolderProjectName + ':/usr/src/mymaven -w /usr/src/mymaven esri/maven-tool:3.8.6-openjdk-11 mvn install'
                }
            }
        }

        

        stage('Resultado de la suma') {
            steps {
                script {
                    def number1
                    def number2
                    def resultado

                    number1 = "$NUMBER_1"
                    number2 = "$NUMBER_2"

                    resultado = number1 + number2

                    println "Resultado de la suma: " + resultado
                }
            }
        }
    }

    post {
        always {
            chuckNorris()
        }
    }
}