#!groovy
fileLoader.withGit(
        'git@github.com:wyndow/jenkins-groovy-lib.git',
        'master', '08a2e627-40ea-43a9-9a8f-d1bbf370139b', '') {
          jenkins = fileLoader.load('jenkins');
          packer = fileLoader.load('packer');
          slack = fileLoader.load('slack');
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
