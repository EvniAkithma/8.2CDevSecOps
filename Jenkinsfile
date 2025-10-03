pipeline {
    agent any

    environment {
        NODE_VERSION = '18.18.0' // Change if you want a different Node.js version
        NODE_DIR = "${WORKSPACE}\\node"
        PATH = "${NODE_DIR}\\node-v${NODE_VERSION}-win-x64;${NODE_DIR}\\node-v${NODE_VERSION}-win-x64\\npm;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/EvniAkithma/8.2CDevSecOps.git'
            }
        }

        stage('Install Node.js') {
            steps {
                bat """
                if not exist "%NODE_DIR%" mkdir "%NODE_DIR%"
                powershell -Command "Invoke-WebRequest -Uri https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-win-x64.zip -OutFile ${NODE_DIR}\\node.zip"
                powershell -Command "Expand-Archive -Path ${NODE_DIR}\\node.zip -DestinationPath ${NODE_DIR}"
                """
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit 0'
                archiveArtifacts artifacts: 'test.log', allowEmptyArchive: true
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit 0'
                archiveArtifacts artifacts: 'coverage/**', allowEmptyArchive: true
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit 0'
                bat 'npm audit --json > npm-audit.json || exit 0'
                archiveArtifacts artifacts: 'npm-audit.json', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo "Build finished: ${currentBuild.currentResult}"
        }
    }
}
