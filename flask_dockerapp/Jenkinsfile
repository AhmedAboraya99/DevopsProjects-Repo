pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', credentialsId: 'Github', credentialsId: 'Github', url: 'https://github.com/AhmedAboraya99/flask_dockerapp.git'

                sh "docker build -t ahmedaboraya99/flask_dockerapp:$env.BUILD_NUMBER ."
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'dockeruser')]) {
                sh "docker login -u $dockeruser -p $password"
                sh "docker push ahmedaboraya99/flask_dockerapp:$env.BUILD_NUMBER"
                }

            }
            
            }
            stage('deploy') {
            steps {
                sh "docker run -d -p 5000:8080 ahmedaboraya99/flask_dockerapp:$env.BUILD_NUMBER"
        
            }
            }
    }
}
