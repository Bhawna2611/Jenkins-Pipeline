pipeline {
    agent any


    tools {
        jdk 'jdk8'
        maven 'maven3'
    }

    stages {
        stage('Check Environment') {
            steps {
                script {
                    if (params.DEPLOY_ENV != 'dev') {
                        error("Pipeline aborted: Only 'dev' environment is allowed to run this pipeline.")
                    } else {
                        echo "Running pipeline in DEV environment"
                    }
                }
            }
        }
        stage('Show Environment') {
            steps {
                echo "Pipeline is running with ${params.DEPLOY_ENV} environment"
            }
        }
        
        stage('Clone Repository') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/opstree/spring3hibernate.git'
            }
        }
        
        stage('Git Leaks') {
            steps {
                 sh '''gitleaks detect \\
                --source . \\
                --report-format json \\
                --report-path gitleaks-report.json \\
                --no-git'''
            }
        }
        
        stage('Maven Clean') {
            steps {
                sh 'mvn clean'
            }
        }
        
        stage('Approval') {
            steps {
                script {
                    def userChoice = input(
                        message: 'Do you want to proceed with the build?',
                        parameters: [
                            choice(
                                name: 'PROCEED',
                                choices: ['yes', 'no'],
                                description: 'Select yes to continue, no to abort'
                            )
                        ]
                    )

                    if (userChoice == 'no') {
                        error "Build aborted by user choice"
                    }
                }
            }
        }

        stage('Build & Test') {
            parallel {
                stage('Compile') {
                    steps {
                        sh 'mvn compile'
                    }
                }

                stage('Unit Test') {
                    steps {
                        sh 'mvn test'
                    }
                }
            }
        }
        
        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Archive') {
            steps {
                sh '''
                mv target/*.war target/app-${BUILD_NUMBER}.war
                '''
                archiveArtifacts artifacts: 'target/app-${BUILD_NUMBER}.war, gitleaks-report.json', followSymlinks: false
            }
        }
    }
    post {
        success {
            emailext(
                subject: "SUCCESS: Build #${env.BUILD_NUMBER} - ${env.JOB_NAME}",
                body: "Good news! The build has succeeded.\n\nCheck details: ${env.BUILD_URL}",
                to: "bhavna123porwal@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "FAILURE: Build #${env.BUILD_NUMBER} - ${env.JOB_NAME}",
                body: "Oops! The build failed.\n\nCheck details: ${env.BUILD_URL}",
                to: "bhavna123porwal@gmail.com"
            )
        }
        aborted {
            emailext(
                subject: "ABORTED: Build #${env.BUILD_NUMBER} - ${env.JOB_NAME}",
                body: "The build was aborted.\n\nCheck details: ${env.BUILD_URL}",
                to: "bhavna123porwal@gmail.com"
                )
            }
        }
    }
