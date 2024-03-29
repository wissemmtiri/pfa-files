pipeline {
    agent any
    environment {
        DOCKER_CREDS = credentials('dockerhub-credentials') 
        // DOCKER_CREDS_USR for username and DOCKER_CREDS_PSW for password
        DOCKER_REPONAME = 'pfa-app-backend'
    }
    stages {
        stage('Dependencies Installation'){
            steps {
                script {
                    echo 'Installing Dependencies ...'
                    try {
                        sh 'npm install'
                    } catch (Exception e) {
                        error("Failed to install dependencies")
                    }
                }
            }
        }
        stage('Static Code Analysis (SCA)'){
            steps {
                script {
                    echo 'Linting and Formatting Code ...'
                    try {
                        sh 'npm run lint'
                        sh 'npm run format'
                    } catch (Exception e) {
                        error("Failed to lint and format code")
                    }
                }
            }
        }
        stage('Test'){
            steps {
                script {
                    echo 'Unit Tests Running ...'
                    try {
                        sh 'npm run test'
                    } catch (Exception e) {
                        error("Failed to run unit tests")
                    }

                    echo 'Integration Tests Running ...'
                    // Uncomment the following line if integration tests are ready
                    // sh 'npm run test:integration'

                    echo 'E2E Tests Running ...'
                    // Uncomment the following line if End-To-End tests are ready
                    // sh 'npm run test:e2e'
                }
            }
        }
        stage('Static Application Security Testing (SAST)'){
            steps {
                script {
                    echo 'Scanning for Vulnerabilities ...'
                    try {
                        sh 'npm audit'
                    } catch (Exception e) {
                        error("Failed to scan for vulnerabilities")
                    }
                    // Further steps may include scanning for vulnerabilities using 
                    // tools like Snyk, SonarQube, etc.
                }
            }
        }
        stage('Build'){
            steps {
                script {
                    echo 'Building the docker image ...'
                    try {
                        sh "docker build -t ${DOCKER_USERNAME}/${DOCKER_REPONAME}:latest ."
                    } catch (Exception e) {
                        error("Failed to build Docker image")
                    }
                }
            }
        }
        stage('Upload'){
            steps {
                script {
                    echo 'Uploading image to DockerHub ...'
                    try {
                        sh "docker login -u $DOCKER_CREDS_USR -p $DOCKER_CREDS_PSW"
                        sh "docker push ${DOCKER_USERNAME}/${DOCKER_REPONAME}:latest"
                        sh 'docker logout'
                    } catch (Exception e) {
                        error("Failed to upload Docker image")
                    }
                }
            }
        }
    }
}