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
            sh 'sleep 70'
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
                      copyArtifacts filter: '**/target/*.jar', fingerprintArtifacts: true, projectName: 'Pull_Request_Artifact_Builder', selector: lastWithArtifacts(), target: '.'
                 }
                 //zip -r artifacts-${env.BUILD_NUMBER}.zip artifacts
                 //sh label: '', script: 'zip -r artifacts-${env.BUILD_NUMBER}.zip artifacts'
                 //zip zipFile: 'artifacts-${env.BUILD_NUMBER}.zip', dir:'artifacts'
                 zip archive: true, dir: 'artifacts', glob: '', overwrite: false, zipFile: env.BUILD_NUMBER
                 sh 'ls -la'
                 
                 //sh label: '', script: 'git add ${env.BUILD_NUMBER}'
                 //sh label: '', script: 'git commit -m "push to git'
                 //sh label: '', script: 'git push https:///${GIT_USER_NAME}:${GIT_USER_PASSWORD}@${GIT_PROJECT_ARTIFACTORY}.git master'
                 //sh label: '', script: '''git add ${env.BUILD_NUMBER}.zip
                 //git commit -m "push to git"
                 //git push https:///${GIT_USER_NAME}:${GIT_USER_PASSWORD}@${GIT_PROJECT_ARTIFACTORY}.git master'''
                 //sh ('git add ${env.BUILD_NUMBER}.zip')
                 //sh ('git commit -m 'Archive')
                 //sh label: '', script: 'git commit -m \'Jar archive #${env.BUILD_NUMBER}\''
                 //sh ('git push https://${GIT_USER_NAME}:${GIT_USER_PASSWORD}@${GIT_PROJECT_ARTIFACTORY}')
                 git status
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

