pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        dockerTool "docker"
        
    }

    stages {
        stage('VCS') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/creepyghost/hello-world-webapp'
            }
        }
        stage('Setup') {
            steps {
                script {
                    sh """
                    pip install -r requirements.txt
                    gunicorn app:app
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: 'c01a453f-fe69-47d4-ad63-81dfaa273d7c',
                        toolName: 'docker') {
                        
                        // Build and Push
                        def echoServerImage = docker.build("creepyghost/java-echoserver:latest");
                        echoServerImage.push();
                    }
                }
            }
        }
    }
    post {
        // If Maven was able to run the tests, even if some of the test
        // failed, record the test results and archive the jar file.
        success {
            echo "Success"
        }
        failure {
            echo "Failure! Duh!"
        }
    }
}
