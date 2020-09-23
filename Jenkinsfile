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
            sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Directory see') {
            steps {
            sh "ls -lat"
            sh"java -jar /home/jenkins/workspace/Pull Request Artifact Builder/website/target/website-0.0.1-SNAPSHOT.jar "
            }
        }
    }
}

