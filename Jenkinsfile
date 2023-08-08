pipeline{
    agent any
    
    stages{
        stage('Copy Code'){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/dushyantkumark/Weather-App.git", branch:"main"
            }
        }
        stage('Build'){
            steps{
                echo "Building the image"
                sh "docker build -t weather_app ."
            }
        }
        stage('Logging & Pushing to DockerHub'){
            steps{
                echo "Pushing the image to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag weather_app ${env.dockerHubUser}/weatherapp:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/weatherapp:latest"
                }
            }
        }
        stage('Deploy'){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }

    post{
        success{
            emailext attachLog: true, body: 'Email send out from Jenkins', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'kumardushyant545@gmail.com'
        }
        failure{
            emailext attachLog: true, body: 'Email send out from Jenkins', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'kumardushyant545@gmail.com'
        }
    }    
}
