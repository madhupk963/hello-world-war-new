pipeline {
    agent {label 'build'}
    stages {
        stage('my Build') {
            steps {
                sh 'ls'
                sh 'docker build -t tomcat_build:${BUILD_VERSION} --build-arg BUILD_VERSION=${BUILD_VERSION} .'
                sh 'helm package ./tomcat --version=${BUILD_VERSION}'
            }
        }  
        stage('publish stage') {
            steps {
                 sh "echo ${BUILD_VERSION}"
                withCredentials([usernamePassword(credentialsId: 'Dockerhub', passwordVariable: 'DockerhubPassword', usernameVariable: 'DockerhubUser')]) {
                sh "docker login -u ${env.DockerhubUser} -p ${env.DockerhubPassword}"
                sh 'docker tag tomcat_build:${BUILD_VERSION} madhusudhanpk/tomcat:${BUILD_VERSION}'
                sh 'docker push madhusudhanpk/tomcat:${BUILD_VERSION}'
                }
            }
        } 
        stage( 'my deploy' ) {
        agent {label 'publish'} 
            steps {
               sh 'helm repo add helm https://madhuhelm.jfrog.io/artifactory/api/helm/helm --username madhu050963@gmail.com --password cmVmdGtuOjAxOjE3MDc5OTQ1MDA6NXNDZFcwaXJlYktaZmdMNnFCT3V5TzUzN3VC'
               sh 'helm repo update'
               sh 'helm repo list'
               sh 'helm upgrade --install tomcat helm/tomcat --version=${BUILD_VERSION} --set selector_matchlabels=tomcat --set deployment_name=tomcat --set replicas=2 --set registry_name=madhusudhanpk --set docker_repo_name=tomcat --set image_tag=${BUILD_VERSION} --set port_name=tomcat --set target_port=8080 --set port=8080 --set favorite.drink=coffee --set favorite.food=pizza'
            }
        }    
    } 
}
