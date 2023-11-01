pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'skinnysydcontainersregistry.azurecr.io/DentistSys'
    }

    stages {
        stage('Clone repository') {
            steps {
                // clone source code from our Git repository.
                checkout scm
            }
        }
        
        stage('JUnit Tests') {
            steps {
                sh './mvnw test'  // Maven Wrapper 
                // sh 'mvn test' 
            }
        }

        stage('Build image') {
            steps {
                    // to Build the Docker image from our app.
                    def app = docker.build(DOCKER_IMAGE_NAME)
            }
        }

        stage('Test image') {
            steps {
                    // to test the Docker image. 
                    //a placeholder for real tests.
                    script {

                        app.inside {
                            sh 'echo "Tests passed"'  // Actual tests to do.
                      }
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    // using Jenkins credentials.
                    docker.withRegistry('https://skinnysydcontainersregistry.azurecr.io', 'azurecr') {
                        // Push the image 
                        app.push()
                    }
                }
            }
        }
    }

    post {
        always {
            // clean up the workspace.
            cleanWs()
        }
    }
}
