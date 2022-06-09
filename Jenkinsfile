pipeline {
    agent none
    stages {
        stage('Build') {
            node('aws')
            steps {
                // Get some code from a GitHub repository
                withCredentials([usernamePassword(credentialsId:"docker-hub",usernameVariable:"username",passwordVariable:"pass")]){
                sh 'docker build . -t ${username}/jenkins_sprints:v1.0'
                sh 'docker login -u ${username} -p ${pass}'
                sh 'docker push ${username}/jenkins_sprints:v1.0'
                }
            }
        }  
        stage ('deploy'){
            node('private-ec2')
            steps{
                withCredentials([usernamePassword(credentialsId:"docker-hub",usernameVariable:"username",passwordVariable:"pass")]){
                sh 'docker login -u ${username} -p ${pass}'
                sh 'docker pull ${username}/jenkins_sprints:v1.0'
                sh 'docker run -p 3000:3000 -d ${username}/jenkins_sprints:v1.0'
                }
            }      
        }
    }
}
