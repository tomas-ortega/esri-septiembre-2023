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

        stage('Verificar Maven') {
            steps {
                sh 'echo "mvn verify"'
                sh '''
                    echo "A"
                    echo "B"
                    echo "C"
                '''
            }
        }

        stage('Build Maven') {
            steps {
                sh 'echo "mvn build"'
            }
        }

        stage('Test Maven') {
            steps {
                sh 'echo "mvn test"'
            }
        }

        stage('Mojito Time') {
            steps {
                sh '''
                    export QUE_TOCA_A_LAS_8="MOJITO"
                    echo "$QUE_TOCA_A_LAS_8"
                '''
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