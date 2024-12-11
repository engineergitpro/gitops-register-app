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

        // stage("Update the Deployment Tags") {
        //     steps {
        //         sh """
        //            cat deployment.yaml
        //            sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
        //            cat deployment.yaml
        //         """
        //     }
        // }

        // stage("Push the changed deployment file to Git") {
        //     steps {
        //         sh """
        //            git config --global user.name "engineergitpro"
        //            git config --global user.email "supriyajer94@gmail.com"
        //            git add deployment.yaml
        //            git commit -m "Updated Deployment Manifest"
        //            git push origin main
        //         """
        //         withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
        //             sh "git push https://github.com/engineergitpro/gitops-register-app main"
        //           //  sh "kubectl apply -f deployment.yaml"
        //         }
        //     }
        // }

        stage('Update Deployment File') {
            environment {
            GIT_REPO_NAME = "gitops-register-app"
            GIT_USER_NAME = "engineergitpro"
        }
        steps {
            withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                sh '''
                    git config user.email "supriyajer94@gmail.com"
                    git config user.name "Supriya Jadhav"
                    git config --global user.email "gitops-register-app"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" gitops-register-app/deployment.yml
                    git add gitops-register-app/deployment.yml
                    git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                    git push https://${github}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                '''
            }
        }
    }
       
      
    }
}
