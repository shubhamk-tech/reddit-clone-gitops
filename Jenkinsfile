pipeline {
    agent any
    environment {
        APP_NAME = "reddit-clone-pipeline"
    }

    stages {
        stage('Cleanup Workspaces') {
            steps {
                cleanWs()
            }
        }
        
        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/shubhamk-tech/reddit-clone-gitops.git'
            }
        }
        
        stage('Update the Deployments Tags') {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """    
            }
        }
        
        stage('Push the changed deployment file to GitHub') {
            steps {
                sh """
                    git config --global user.name "shubhamk-tech"
                    git config --global user.email "shubhkal01@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployemnt Manifests"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-cred', gitToolName: 'Default')]) {
                    sh "git push https://github.com/shubhamk-tech/reddit-clone-gitops.git main"
                }
            }
        }
    }
}
