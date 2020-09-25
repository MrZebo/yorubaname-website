pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Test Build') {
            steps {
            sh 'mvn -B -DskipTests  clean install'
            dir('website') {
               sh 'nohup mvn spring-boot:run -Dspring.profiles.active=inmemory &'
              }
            sh 'sleep 60'
            sh 'http_code=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8081')
            sh 'echo $http_code'
            }
        }
    }
}

