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
          }
        }
        stage('test') {
          steps{
            script{
            dir('website') {
               sh 'nohup mvn spring-boot:run -Dspring.profiles.active=inmemory &'
            }
            sh 'sleep 60'
            sh 'curl -s -o /dev/null -w "%{http_code}" http://localhost:8081'
            sh 'env'
          }
         }
          post {
                success {
                 echo "Success"
                 archiveArtifacts(artifacts: '**/target/*.jar', allowEmptyArchive: true)
                 sh 'git config --global user.email "test@gmail.com"'
                 sh 'git config --global user.name "MrZebo"'
                 sh("git tag -a master-${env.BUILD_NUMBER} -m 'Jenkins'")
                 sh('git push https://${GIT_USER_NAME}:${GIT_USER_PASSWORD}@${GIT_PROJECT_REPO} --tags')
                 
                 dir('website/target') {
                   sh 'ls -la $JENKINS_HOME/jobs/${JOB_NAME}/builds/${BUILD_NUMBER}/'
                   sh 'pwd'
                   sh 'zip -r artifacts-${env.BUILD_NUMBER}.zip **/target/*.jar'
                 }
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

