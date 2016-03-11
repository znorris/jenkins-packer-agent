#!groovy
fileLoader.withGit(
        'https://source.developers.google.com/p/automated-builder-wyndow/',
        'master', 'c1a38b22-0cb5-4f54-a7a9-06336b609be3', '') {
          jenkins = fileLoader.load('src/com/wyndow/build/jenkins');
          packer = fileLoader.load('src/com/wyndow/build/packer');
          slack = fileLoader.load('src/com/wyndow/build/slack');
        }

def cleanJobName = env.JOB_NAME.replaceAll('%2F','/')

node('gcp-packer') {
  stage 'Development'
  try{
    checkout scm
  } catch (Exception e){
    slack.send("${cleanJobName} - <${env.BUILD_URL}|#${env.BUILD_NUMBER}> Failed Development Stage", '',
          'd83ac413-e303-4dce-bc3a-58fcb86e19d9')
    throw e
  }


  stage 'Build'
  try{
    packer.build('./packer.json', 'jenkins-packer-agent')
    slack.send("${cleanJobName} - <${env.BUILD_URL}|#${env.BUILD_NUMBER}> Completed\n${packer.imageBuilt()}", '',
          'd83ac413-e303-4dce-bc3a-58fcb86e19d9')
  } catch (Exception e){
    slack.send("${cleanJobName} - <${env.BUILD_URL}|#${env.BUILD_NUMBER}> Failed Build Stage", '',
          'd83ac413-e303-4dce-bc3a-58fcb86e19d9')
    throw e
  }
}
