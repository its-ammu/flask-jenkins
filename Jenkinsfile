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
                    docker tag ${USERNAME}/flask:build-${BUILD_ID} flaskapp:build-${BUILD_ID}
                    docker push ${USERNAME}/flask:build-${BUILD_ID}
                    """
                }
        
            }
        }

        stage('DEPLOY'){
            steps{
                
                echo "DEPLOYING THE APP... "
                
                sh "docker run -p -d 3000:5000 ${USERNAME}/flask:build-${BUILD_ID}"

                echo "Succesfully deployed and running in port 3000"
                
            }
        }

    }   
}