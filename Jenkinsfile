pipeline {
    agent any
    environment {
        IMAGE_TAG = "$BUILD_NUMBER"
        DOCKERHUB_USERNAME = "aakkiiff"
        GIT_REPO = "" 

        AUTH_APP_NAME = "auth"
        UI_APP_NAME = "ui"
        WEATHER_APP_NAME = "weather"

        AUTH_APP_IMAGE = "${DOCKERHUB_USERNAME}/${AUTH_APP_NAME}"
        UI_APP_IMAGE = "${DOCKERHUB_USERNAME}/${UI_APP_NAME}"
        WEATHER_APP_IMAGE = "${DOCKERHUB_USERNAME}/${WEATHER_APP_NAME}"
    }

    stages{
        stage('CLEANUP WORKSPACE'){
            steps{
                script{
                    cleanws()
                }
            }
        }

        stage("CHECKOUT GIT REPO"){
            steps{
                git "${GIT_REPO}"
            }
        }

        stage("BUILD DOCKER IMAGES"){
            steps{
                sh'docker build --no-cache -t ${AUTH_APP_IMAGE}:${IMAGE_TAG} -t ${AUTH_APP_IMAGE}:latest -f ./auth/Dockerfile ./auth'
                sh'docker build --no-cache -t ${UI_APP_IMAGE}:${IMAGE_TAG} -t ${UI_APP_IMAGE}:latest -f ./UI/Dockerfile ./UI'
                sh'docker build --no-cache -t ${WEATHER_APP_IMAGE}:${IMAGE_TAG} -t ${WEATHER_APP_IMAGE}:latest -f ./weather/Dockerfile ./weather'
            }
        }

        stage("PUSH DOCKER IMAGES TO DOCKERHUB"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
                    sh'docker login -u ${USER_NAME} -p ${PASSWORD}'

                    sh'docker push ${AUTH_APP_IMAGE}:${IMAGE_TAG}'
                    sh'docker push ${AUTH_APP_IMAGE}:latest'

                    sh'docker push ${UI_APP_IMAGE}:${IMAGE_TAG}'
                    sh'docker push ${UI_APP_IMAGE}:latest'
                    
                    sh'docker push ${WEATHER_APP_IMAGE}:${IMAGE_TAG}'
                    sh'docker push ${WEATHER_APP_IMAGE}:latest'
                }
            }
        }
    }
}
