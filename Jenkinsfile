pipeline {
    agent any
    
     environment {
        SERVICE_NAME = "maven"
        ORGANIZATION_NAME = "muhammadadel8"
        DOCKERHUB_USERNAME = "muhammadadel8"
        REPOSITORY_TAG = "${DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
    }
    
    tools {
        maven "maven" 
   }
   
   stages {
       stage ('git checkout') {
           steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']],userRemoteConfigs: [[url: 'https://github.com/MuhammadAdel612/maven-web-application.git']]])
           }
       }
       stage ('build maven and copy war file') {
           steps {
               sh '''
               mvn clean package
              
               '''
               
           }
       }
       
        stage ('Build and Push Image') {
            steps {
                 withDockerRegistry([credentialsId: 'docker-id', url: ""]) {
                   sh 'docker build -t ${REPOSITORY_TAG} .'
                   sh 'docker push ${REPOSITORY_TAG}'          
            }
          }
       }
       
       stage ('Deploy to Cluster') {
            steps {
                sh "cat maven-deploy.yml | sed 's/{{BUILD_NUMBER}}/${BUILD_NUMBER}/g' | kubectl apply -f -"
                }
            }
}
}
