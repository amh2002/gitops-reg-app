pipeline {
    agent { label "Jenkins-Agent" }
    environment {
        APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/amh2002/gitops-reg-app.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "amh2002"
                   git config --global user.email "amh.2002@outlook.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """

                withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                    sh "git push https://${TOKEN}@github.com/amh2002/gitops-reg-app main"
                }
            }
        }
        
    } // <--- THIS IS THE MISSING BRACKET THAT CLOSES 'stages'
} // <--- This closes 'pipeline'