pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('build') {
          steps{
            echo "My branch is: ${env.BRANCH_NAME}"
            sh 'mvn -B -DskipTests  clean install'
            dir('website') {
               sh 'nohup mvn spring-boot:run -Dspring.profiles.active=inmemory &'
             }
          }
        }
        stage('test') {
          steps{
            sh 'sleep 60'
            sh 'http_code=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8081)'
            def status = sh 'curl -s -o /dev/null -w "%{http_code}" http://localhost:8081'
            sh 'echo status'
          }
        }
        stage('deploy') {
            // ...
        }
    }
}

