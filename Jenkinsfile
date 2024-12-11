pipeline {
    agent { label "jenkins-agent" }
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
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/engineergitpro/gitops-register-app.git'
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
                   git config --global user.name "engineergitpro"
                   git config --global user.email "supriyajer94@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                   git push origin main
                """
                // withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                //  // sh "git push https://github.com/engineergitpro/gitops-register-app main"
                //   //  sh "kubectl apply -f deployment.yaml"
                // }
            }
        }
        stage("deploy"){
            steps{
                sh  "kubectl apply -f deployment.yaml"'
            }
        }
        
      
    }
}
