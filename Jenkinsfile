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
            script{
            sh 'sleep 60'
            def response = sh 'curl -s -o /dev/null -w "%{http_code}" http://localhost:8081'
            echo "${response}"
          }
         }
          post {
                success {
                 echo "Success"
                }
                failure {
                 echo "Failure"
                }
        }
        stage('deploy') {
          steps{
             echo "Deploy to aws servers via terraform and ansible"
          }
        }
    }
}

