pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Test Build') {
            steps {
            sh 'mvn -B -X -DskipTests  clean install'
            dir('website') {
               sh 'nohup mvn spring-boot:run -Dspring.profiles.active=inmemory &'
              }
            sh 'sleep 60'
            sh 'curl -s -o /dev/null -w "%{http_code}" http://localhost:8081'
            if(http_code == 200){
              echo 'WOOORK!!!!11111'}
            esle{
              echo 'FAIL :('
            }
            }
        }
    }
}

