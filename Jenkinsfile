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
                   git branch: 'main', credentialsId: 'github2', url: 'https://github.com/francis0516/gitops-register-app'
               }
        }

        stage('Update the Deployment Tags') {
            steps {
                script {
                    def BUILD_TAG = env.BUILD_NUMBER  // or use env.GIT_COMMIT
                    sh """
                        sed -i 's|image: francis0516/register-app-pipeline.*|image: francis0516/register-app-pipeline:${BUILD_TAG}|g' deployment.yaml
                        cat deployment.yaml
                    """
                }
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "francis0516"
                   git config --global user.email "henogez@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github2', gitToolName: 'Default')]) {
                  sh "git push https://github.com/francis0516/gitops-register-app main"
                }
            }
        }
      
    }
}
