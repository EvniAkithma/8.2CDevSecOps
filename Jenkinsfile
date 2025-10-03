pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { git branch: 'main', url: 'https://github.com/EvniAkithma/8.2CDevSecOps.git' }
    }
    stage('Install Dependencies') {
      steps { sh 'npm install' }
    }
    stage('Run Tests') {
      steps {
        sh 'npm test 2>&1 | tee test.log || true'
        archiveArtifacts artifacts: 'test.log', allowEmptyArchive: true
      }
    }
    stage('Generate Coverage Report') {
      steps {
        sh 'npm run coverage 2>&1 | tee coverage.log || true'
        archiveArtifacts artifacts: 'coverage/**', allowEmptyArchive: true
      }
    }
    stage('NPM Audit (Security Scan)') {
      steps {
        sh 'npm audit 2>&1 | tee audit.log || true'
        sh 'npm audit --json > npm-audit.json || true'
        archiveArtifacts artifacts: 'audit.log,npm-audit.json', allowEmptyArchive: true
      }
    }
  }
  post { always { echo "Build finished: ${currentBuild.currentResult}" } }
}
