pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/EvniAkithma/8.2CDevSecOps.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }
    stage('Run Tests') {
      steps {
        bat 'npm test 2>&1 | tee test.log || exit 0'
        archiveArtifacts artifacts: 'test.log', allowEmptyArchive: true
      }
    }
    stage('Generate Coverage Report') {
      steps {
        bat 'npm run coverage 2>&1 | tee coverage.log || exit 0'
        archiveArtifacts artifacts: 'coverage/**', allowEmptyArchive: true
      }
    }
    stage('NPM Audit (Security Scan)') {
      steps {
        bat 'npm audit 2>&1 | tee audit.log || exit 0'
        bat 'npm audit --json > npm-audit.json || exit 0'
        archiveArtifacts artifacts: 'audit.log,npm-audit.json', allowEmptyArchive: true
      }
    }
  }
  post { 
    always { 
      echo "Build finished: ${currentBuild.currentResult}" 
    } 
  }
}

