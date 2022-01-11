pipeline {
    agent any
    tools {
        dockerTool 'docker'
    }
    stages{

        stage('BUILD'){
            steps{

                echo "BUILDING THE IMAGE... "
                
                sh "docker build -t flaskapp:build-${BUILD_ID} ."
                
            }
        }

        stage('PUSH TO REPO'){
            steps{

                echo "PUSHING THE IMAGE TO REPO... "

                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh """
                    docker login --username ${USERNAME} -p "${PASSWORD}"
                    docker tag flaskapp:build-${BUILD_ID} ${USERNAME}/flaskapp:build-${BUILD_ID}
                    docker push ${USERNAME}/flaskapp:build-${BUILD_ID}
                    """
                }
        
            }
        }

        stage('DEPLOY'){
            steps{
                echo "DEPLOYING THE APP..."
                sh "docker run -p 3000:5000 -d ${USERNAME}/flaskapp:build-${BUILD_ID}"
                echo "Succesfully deployed and running in port 3000"
            }
        }

    }   
}