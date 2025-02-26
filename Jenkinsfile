pipeline {
    agent any
   
    stages {
        stage('Clone Repository') {
            steps {
                // Clean workspace before starting
                cleanWs()
               
                // Echo the current directory for verification
                sh 'echo "Current directory: $PWD"'
               
                // Clone the repository into workspace instead of Jenkins home
                sh '''
                    if [ -d "my_python_project" ]; then
                        echo "Directory already exists, removing it"
                        rm -rf my_python_project
                    fi
                   
                    git clone https://github.com/jineshranawatcode/my_python_project.git
                   
                    echo "Repository cloned successfully!"
                    ls -la my_python_project
                '''
            }
        }
       
        stage('Verify Clone') {
            steps {
                // Verify in the workspace where we have permissions
                sh '''
                    cd my_python_project
                    ls -la
                    git status
                '''
            }
        }
       
        stage('Build Wheel') {
            steps {
                // Change directory to the cloned repository
                dir('my_python_project') {
                    sh 'pip install build'
                    sh 'python3 -m build --wheel'
                }
            }
        }
       
        stage('Build Docker Image') {
            steps {
                // Change directory to the cloned repository
                dir('my_python_project') {
                    sh 'docker build -t my-python-app:latest .'
                }
            }
        }
       
        stage('Deploy') {
            steps {
                sh 'docker stop my-python-container || true'
                sh 'docker rm my-python-container || true'
                sh 'docker run -d --name my-python-container my-python-app:latest'
            }
        }
    }
   
    post {
        success {
            echo 'Repository cloned, built, and deployed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}