// Jenkinsfile (à la racine du dépôt)
pipeline {
  /* 1️⃣  Tout le pipeline tourne à l’intérieur d’un conteneur python:3.10-slim
        Le socket Docker du host étant monté, on peut quand même builder nos images ensuite. */
  agent {
    docker {
      image 'python:3.10-slim'
      args  '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  stages {
    stage('Tests') {
      steps {
        sh '''
          pip install --no-cache-dir -r art-explorer/requirements.txt pytest
          cd art-explorer
          pytest -q
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

  /* 2️⃣  On retire JUnit et Mail pour l’instant (tu pourras les remettre plus tard). */
}