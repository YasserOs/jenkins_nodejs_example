pipeline {
    agent none
    stages {
        stage('Build') {
           
            steps {
                node('aws'){
                    withCredentials([usernamePassword(credentialsId:"docker-hub",usernameVariable:"username",passwordVariable:"pass")]){
                    sh 'docker build . -f dockerfile -t ${username}/jenkins_sprints:v1.0'
                    sh 'docker login -u ${username} -p ${pass}'
                    sh 'docker push ${username}/jenkins_sprints:v1.0'
                    }
                }
                // Get some code from a GitHub repository
                
            }
        }  
        stage ('deploy'){
            
            steps{
                node('private-ec2'){
                    withCredentials([usernamePassword(credentialsId:"docker-hub",usernameVariable:"username",passwordVariable:"pass")]){
                    sh 'docker login -u ${username} -p ${pass}'
                    sh 'docker pull ${username}/jenkins_sprints:v1.0'
                    sh 'docker run -p 3000:3000 -d ${username}/jenkins_sprints:v1.0'
                    }
                }
                
                
            }      
        }
    }
}
