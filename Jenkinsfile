pipeline {
    agent any
    tools {
        // Optional: if you configured NodeJS in Jenkins via NodeJS plugin, replace 'NodeJS-18' with your tool name
        nodejs 'NodeJS-18'
    }
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
                // Run tests, output to test.log, do not fail the pipeline if tests fail
                bat 'npm test > test.log 2>&1 || exit 0'
                archiveArtifacts artifacts: 'test.log', allowEmptyArchive: true
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage > coverage.log 2>&1 || exit 0'
                archiveArtifacts artifacts: 'coverage/**', allowEmptyArchive: true
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit > audit.log 2>&1 || exit 0'
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

