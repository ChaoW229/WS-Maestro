pipeline {
    agent any

    parameters {
        string(name: 'DEVICE_ID', defaultValue: 'SD53150494', description: 'Android device id')
        choice(name: 'TEST_TAG', choices: ['smoke', 'regression'], description: 'Maestro test tag')
    }

    environment {
        MAESTRO = 'D:\\MaestroCLI\\bin\\maestro.bat'
        ARTIFACT_DIR = 'artifacts'
    }

    stages {
        stage('Prepare Artifacts') {
            steps {
                bat '''
                if exist %ARTIFACT_DIR% rmdir /s /q %ARTIFACT_DIR%
                mkdir %ARTIFACT_DIR%
                '''
            }
        }

        stage('Check Environment') {
            steps {
                bat 'git --version'
                bat 'adb version'
                bat '"%MAESTRO%" --version'

                bat 'adb version > %ARTIFACT_DIR%\\adb-version.txt'
                bat '"%MAESTRO%" --version > %ARTIFACT_DIR%\\maestro-version.txt'
            }
        }

        stage('Check Devices') {
            steps {
                bat 'adb kill-server'
                bat 'adb start-server'
                bat 'adb devices -l'

                bat 'adb devices -l > %ARTIFACT_DIR%\\device-info.txt'
            }
        }

        stage('Write Build Metadata') {
            steps {
                bat '''
                echo JOB_NAME=%JOB_NAME%> %ARTIFACT_DIR%\\execution-meta.txt
                echo BUILD_NUMBER=%BUILD_NUMBER%>> %ARTIFACT_DIR%\\execution-meta.txt
                echo DEVICE_ID=%DEVICE_ID%>> %ARTIFACT_DIR%\\execution-meta.txt
                echo TEST_TAG=%TEST_TAG%>> %ARTIFACT_DIR%\\execution-meta.txt
                echo WORKSPACE=%WORKSPACE%>> %ARTIFACT_DIR%\\execution-meta.txt
		
		git rev-parse HEAD > %ARTIFACT_DIR%\\git-commit.txt
		git log -1 --pretty=oneline > %ARTIFACT_DIR%\\git-last-commit.txt
                '''
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
                bat '"%MAESTRO%" --device %DEVICE_ID% test . --include-tags=%TEST_TAG% --debug-output %ARTIFACT_DIR%\\maestro-debug'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'artifacts/**/*', allowEmptyArchive: true
        }
    }
}