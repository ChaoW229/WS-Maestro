pipeline {
    agent any

    parameters {
        string(name: 'DEVICE_ID', defaultValue: 'SD53150494', description: 'Android device id')
        choice(name: 'TEST_TAG', choices: ['smoke', 'regression'], description: 'Maestro test tag')
    }

    environment {
        MAESTRO = 'D:\\MaestroCLI\\bin\\maestro.bat'
    }

    stages {
        stage('Check Environment') {
            steps {
                bat 'git --version'
                bat 'adb version'
                bat '"%MAESTRO%" --version'
            }
        }

        stage('Check Devices') {
            steps {
                bat 'adb kill-server'
                bat 'adb start-server'
                bat 'adb devices -l'
            }
        }

        stage('Clean ADB Forward') {
            steps {
                bat 'adb forward --remove-all'
                bat 'adb reverse --remove-all'
            }
        }

        stage('Run Maestro') {
            steps {
                bat '"%MAESTRO%" --device %DEVICE_ID% test . --include-tags=%TEST_TAG% --debug-output maestro-debug'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'maestro-debug/**/*', allowEmptyArchive: true
        }
    }
}s