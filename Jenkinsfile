pipeline {
    agent {
        dockerContainer {
            image 'mosazhaw/jenkins-agent:jdk21-1.3'
            dockerHost 'tcp://host.docker.internal:2375'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Starting build..."'
                dir('backend') {
                    sh 'chmod +x ./gradlew'
                    sh './gradlew test'
                }
                jacoco()
                junit testResults: '**/test-results/test/*.xml'
            }
        }

        stage('Frontend Lint') {
            tools {
                nodejs 'NodeJS 23.11.0'
            }
            steps {
                dir('frontend') {
                    sh '''
                        npm install
                        npm run lint:html
                    '''
                }
            }
        }

        stage('SonarQube Backend') {
            steps {
                withCredentials([string(credentialsId: 'Sonarqube-Backend', variable: 'TOKEN')]) {
                    dir('backend') {
                        sh '''
                            ./gradlew sonar \
                              -Dsonar.projectKey=DevOpsDemo-Backend \
                              -Dsonar.projectName="DevOpsDemo-Backend" \
                              -Dsonar.host.url=http://host.docker.internal:9000 \
                              -Dsonar.token=${TOKEN}
                        '''
                    }
                }
            }
        }

        stage('SonarQube Frontend') {
            tools {
                nodejs 'NodeJS 23.11.0'
            }
            steps {
                withCredentials([string(credentialsId: 'Sonarqube-Frontend', variable: 'TOKEN')]) {
                    dir('frontend') {
                        sh '''
                            npm install -g sonar-scanner
                            sonar-scanner \
                              -Dsonar.projectKey=DevOpsDemo-Frontend \
                              -Dsonar.projectName="DevOpsDemo-Frontend" \
                              -Dsonar.host.url=http://host.docker.internal:9000 \
                              -Dsonar.token=${TOKEN}
                        '''
                    }
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                dir('backend') {
                    withCredentials([usernamePassword(credentialsId: 'DockerHubCred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                            export DOCKER_HOST=tcp://host.docker.internal:2375
                            docker build -f docker.dockerfile -t michaelmisa/node-web-app:latest .
                            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                            docker push michaelmisa/node-web-app:latest
                        '''
                    }
                }
            }
        }

        stage('Trigger Render Deployment') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'renderToken', variable: 'KEY')]) {
                        sh "curl $KEY"
                    }
                }
            }
        }
    }
}
