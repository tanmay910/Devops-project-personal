pipeline {    
    agent any 
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {   
        stage('Checkout') {
            steps {
                git credentialsId: 'git-credentials', url: 'https://github.com/tanmay910/Devops-project-personal.git'
            }

        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }

          stage('SonarQube Analsyis') {
            steps {
                withSonarQubeEnv('sonar-qube-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Devops-project -Dsonar.projectKey=Devops-project \
                            -Dsonar.java.binaries=. '''
                }
            }
        }
        stage('Quality Gate') {

            steps {
                script {
                  waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token' 
                }
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Publish to Nexus') {
            steps {
                withMaven(globalMavenSettingsConfig: 'global-setting', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                           sh 'mvn deploy'
                }
            }
        }
        stage('Build and Tag Docker Image') {
            script {
                withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                    sh 'docker build -t tanmay910/devops-project:latest .'
            }
               
            }
        }
         stage('Docker image Scan') {
            steps {
                sh "trivy image --format table -o trivy-image-report.html tanmay910/devops-project:latest"
            }
        }
         stage('Push Docker Image') {
            script {
                withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                    sh 'docker push tanmay910/devops-project:latest'
            }
               
            }
        }
        stage('Push Docker Image') {
            steps{
            withKubeConfig(caCertificate: ", clusterName: 'kubernetes', contextName: ", credentialsid: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false,
                    serverUrl: 'https://172.31.8.146:6443') {
                sh 'kubectl apply -f deployment.yaml'

            }
               
            }
        }

        stage('Push Docker Image') {
            steps{
            withKubeConfig(caCertificate: ", clusterName: 'kubernetes', contextName: ", credentialsid: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false,
                    serverUrl: 'https://172.31.8.146:6443') {
                sh 'kubectl apply -f deployment.yaml'

            }
               
            }
        }
        post{
            always{
                script{
                    def jobName = env.JOB_NAME
                    def buildNumber = env.BUILD_NUMBER
                    def pipelineStatus = currentBuild.result ?: 'UNKNOWN'
                    def bannerColor = pipelineStatus.toUpperCase() == 'SUCCESS' ? 'green' : 'red'

                    def body =  """
                       <html> 
                        <body>
                            <div style="border: 4px solid ${bannerColor}; padding: 10px;">
                            <h2>${jobName} - Build ${buildNumber}</h2> 
                            <div style="background-color: ${bannerColor}; padding: 10px;">
                                <h3 style="color: white;">Pipeline Status: ${pipelineStatus.toUpperCase()}</h3>
                            </div>
                            <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
                            </div>
                        </body> 
                    </html>
                    """
                    emailext (
                            body: body,
                            to: 'mahajantanmay910@gmail.com',
                            from: 'jenkins@example.com',
                            replyTo: 'jenkins@example.com',
                            mimeType: 'text/html',
                            attachmentsPattern: 'trivy-image-report.html'
                    )



                    
                }
            }
         
        }
    }

