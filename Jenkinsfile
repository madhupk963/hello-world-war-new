pipeline {
    agent {label 'sla'}
    stages {
        stage('my Build') {
            steps {
                sh "echo ${BUILD_VERSION}"
                sh 'docker build -t tomcat_build:${BUILD_VERSION} --build-arg BUILD_VERSION=${BUILD_VERSION} .'
            }
        }  
        stage('publish stage') {
            steps {
                sh "echo ${BUILD_VERSION}"
                sh 'docker login -u prajwal1327 -p Prajwal@1'
                sh 'docker tag tomcat_build:${BUILD_VERSION} prajwal1327/mytomcat:${BUILD_VERSION}'
                sh 'docker push prajwal1327/mytomcat:${BUILD_VERSION}'
            }
        } 
        stage( 'my deploy' ) {
        agent {label 'ansible'} 
            steps {
               sh 'docker pull prajwal1327/mytomcat:${BUILD_VERSION}'
               sh 'docker rm -f mytomcat'
               sh 'docker run -d -p 8080:8080 --name mytomcat prajwal1327/mytomcat:${BUILD_VERSION}'
            }
        }    
    } 
}
