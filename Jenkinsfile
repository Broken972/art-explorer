pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Tests') {
      steps {
        sh 'pip install -r art-explorer/requirements.txt'
        sh 'pytest -q'
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

  post {
    always {
      junit '**/tests/*.xml'   // si tu ajoutes --junit-xml à pytest plus tard
    }
    failure {
      mail to: 'ton.mail@exemple.com',
           subject: "❌ Build #${env.BUILD_NUMBER} FAILED",
           body: "Consulte Jenkins pour le log."
    }
  }
}