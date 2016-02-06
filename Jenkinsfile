#!groovy
fileLoader.withGit(
        'https://source.developers.google.com/p/automated-builder-wyndow/',
        'master', 'c1a38b22-0cb5-4f54-a7a9-06336b609be3', '') {
          jenkins = fileLoader.load('src/com/wyndow/build/jenkins');
          packer = fileLoader.load('src/com/wyndow/build/packer');
          slack = fileLoader.load('src/com/wyndow/build/slack');
        }

def cleanJobName = env.JOB_NAME.replaceAll('%2F','/')

slack.send("${cleanJobName} - <${env.BUILD_URL}|#${env.BUILD_NUMBER}> Started", '',
      'd83ac413-e303-4dce-bc3a-58fcb86e19d9')

node('gcp-packer') {
  stage 'Development'
  checkout scm

  stage 'Build Image'
  packer.build('./packer.json', 'jenkins-packer-agent',
        'debian:jessie',
        'd8610486-c78a-4cfc-8b5d-09b8011a30cd')
}

slack.send("${cleanJobName} - <${env.BUILD_URL}|#${env.BUILD_NUMBER}> Completed\n${imageBuilt}", '',
      'd83ac413-e303-4dce-bc3a-58fcb86e19d9')
