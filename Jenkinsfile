pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Test Stage') {
            steps {
                git branch: 'master',
                url: 'https://github.com/MrZebo/yorubaname-website.git'
            sh "ls -lat"
            }
        }
        stage('Test Build') {
            steps {
            sh 'mvn -B -X -DskipTests  clean install'
            dir('website') {
               sh 'nohup mvn spring-boot:run -Dspring.profiles.active=inmemory &'
              }
            sh 'sleep 60'
            sh 'curl http://localhost:8081'
            sh 'curl -X POST localhost:8081/actuator/shutdown'
            }
        }
    }
}

