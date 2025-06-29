pipeline {
  agent any

  stages {
    stage('Tests') {
      steps {
        sh '''
          pip install --no-cache-dir -r art-explorer/requirements.txt pytest
          cd art-explorer && pytest -q
        '''
      }
    }

    stage('Build image') {
      steps {
        script {
          def tag = "art-explorer:${env.BUILD_NUMBER}"
          sh "docker build -t ${tag} art-explorer"
        }
      }
    }
  }

  /*  Notifications Discord  */
  post {
    success {
      discordSend(
        webhookURL: credentials('discord-webhook'),
        title: "✅ Build #${env.BUILD_NUMBER} – SUCCEEDED",
        description: "Branch *${env.GIT_BRANCH}*\nDuration : ${currentBuild.durationString}"
      )
    }
    failure {
      discordSend(
        webhookURL: credentials('discord-webhook'),
        title: "❌ Build #${env.BUILD_NUMBER} – FAILED",
        description: "Branch *${env.GIT_BRANCH}* – vérifie la console Jenkins.",
        result: 'FAILURE'
      )
    }
  }
}