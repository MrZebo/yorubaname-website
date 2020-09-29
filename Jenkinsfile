pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('build') {
          steps{
            sh 'mvn -B -DskipTests  clean install'
          }
        }
        stage('test') {
          steps{
            script{
            dir('website') {
               sh 'nohup mvn -X spring-boot:run -Dspring.profiles.active=inmemory &'
            }
            sh 'sleep 60'
            sh 'curl -s -o /dev/null -w "%{http_code}" http://localhost:8081'
            sh 'env'
          }
         }
          post {
                success {
                 echo "Success"
                 archiveArtifacts(artifacts: '**/target/*.jar', allowEmptyArchive: true , onlyIfSuccessful: true)
                 sh 'git config --global user.email "test@gmail.com"'
                 sh 'git config --global user.name "MrZebo"'
                 sh ("git tag -a master-${env.BUILD_NUMBER} -m 'Jenkins'")
                 sh ('git push https://${GIT_USER_NAME}:${GIT_USER_PASSWORD}@${GIT_PROJECT_REPO} --tags')
                 sh 'mkdir artifacts'
                 dir('artifacts'){
                   copyArtifacts filter: '**/target/*.jar', fingerprintArtifacts: true, projectName: 'Pull_Request_Artifact_Builder', selector: specific("${env.BUILD_NUMBER}"), target: '.'
                 }

                 sh 'ls -la artifacts'
                 //sh 'wget -r -np -l 1 -A zip --auth-no-challenge --http-user=admin --http-password=vU3KBTHvD9  https://borisdevops.tk/job/Pull_Request_Artifact_Builder/lastSuccessfulBuild/artifact/*zip*/archive.zip' 
                 //sh 'wget -qO- http://10.7.240.162:8080/lastSuccessfulBuild/artifact/*zip*/archive.zip'
                 //sh 'wget -qO- jenkins_url/job/job_name/lastSuccessfulBuild/${env.BUILD_NUMBER}' 
                 //git add ${JOB_URL}/lastSuccessfulBuild/artifact/zip/archive.zip
                 //git status
                 //git add /home/jenkins/workspace/Pull Request Artifact Builder/lastSuccessfulBuild/artifact/*zip*/archive-${env.BUILD_NUMBER}.zip
                }
                failure {
                 echo "Failure"
                }
        }
        }
        stage('deploy') {
          steps{
             echo "Deploy to aws servers via terraform and ansible"
          }
        }
    }
}

