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
            sh 'mvn clean install'
            dir('website') {
               sh 'mvn spring-boot:run -Dspring.profiles.active=inmemory'
              }
            }
        }
    }
}

