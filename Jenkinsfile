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
                 //zip -r artifacts.zip archive
                 //sh label: '', script: 'python -c "import shutil;shutil.make_archive(\'artifacts-${env.BUILD_NUMBER}\',\'zip\',root_dir=\'.\', base_dir=\'archive\')"'
                 //sh 'cd .. && ls -la && pwd && cd .. && ls -la && pwd && cd .. && ls -la && pwd'
                 //https://borisdevops.tk/job/Pull_Request_Artifact_Builder/lastSuccessfulBuild/artifact/zip/archive.zip
                 //zip archive: true, dir: '.', glob: '', zipFile: 'build-artifacts-${env.BUILD_NUMBER}.zip'
                 sh 'git config --global user.email "test@gmail.com"'
                 sh 'git config --global user.name "MrZebo"'
                 sh ("git tag -a master-${env.BUILD_NUMBER} -m 'Jenkins'")
                 sh ('git push https://${GIT_USER_NAME}:${GIT_USER_PASSWORD}@${GIT_PROJECT_REPO} --tags') 
                 git add ${JOB_URL}/lastSuccessfulBuild/artifact/zip/archive-${env.BUILD_NUMBER}.zip
                 //git status
                 //git add /home/jenkins/workspace/Pull Request Artifact Builder/lastSuccessfulBuild/artifact/*zip*/archive-${env.BUILD_NUMBER}.zip
                }
                failure {
                 echo "Failure"
                 //                sh 'cd .. && ls -la && pwd && cd .. && ls -la && pwd && cd .. && ls -la && pwd'
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

