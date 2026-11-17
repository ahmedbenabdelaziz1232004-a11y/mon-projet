pipeline {
  agent any
  options { timestamps() }
  environment {
    // ⚠️ REMPLACE "ahmed" PAR TON PSEUDO DOCKER HUB
    IMAGE = 'ahmed/monapp' 
    TAG = "build-${env.BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Docker Build') {
      steps {
        bat 'docker version'
        bat "docker build -t %IMAGE%:%TAG% ."
      }
    }
    stage('Smoke Test') {
      steps {
        bat '''
          docker rm -f monapp_test 2>nul || ver > nul
          docker run -d --name monapp_test -p 8082:80 %IMAGE%:%TAG%
          ping -n 3 127.0.0.1 > nul
          curl -I http://localhost:8082 | find "200 OK"
          docker rm -f monapp_test
        '''
      }
    }
    stage('Push (Docker Hub)') {
      steps {
        // On ajoutera les identifiants Docker Hub plus tard, on va d'abord tester le Build
        bat 'echo "Le push sera configuré à l\'étape suivante"'
      }
    }
  }
}