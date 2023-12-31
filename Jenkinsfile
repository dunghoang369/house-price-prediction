pipeline {
    agent any // worker will run step, jenkin will choose any agent availabel, agent can be a node, docker image

    options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5')) // only keep logs within 5 days or runs latest
        timestamps() // timestamp of each job
    }

    environment{
        registry = 'dunghoang99/house-price-prediction-api' // change name
        registryCredential = 'dockerhub'      
    }

    stages {
        stage('Test') {
            agent {
                docker {
                    image 'python:3.8' 
                }
            }
            steps {
                echo 'Testing model correctness..'
                sh 'pip install -r requirements.txt && pytest'
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building image for deployment..'
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                    echo 'Pushing image to dockerhub..'
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying models..' 
                echo 'Running a script to trigger pull and start a docker container'
            }
        }
    }
}