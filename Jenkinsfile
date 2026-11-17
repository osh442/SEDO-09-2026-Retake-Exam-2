// Declarative pipeline: build the Homies solution and run all tests
// when changes are pushed to the main branch.

def run(String cmd) {
    if (isUnix()) {
        sh cmd
    } else {
        bat cmd
    }
}

pipeline {
    agent any

    triggers {
        // Poll the repository for new commits on main every 2 minutes
        pollSCM('H/2 * * * *')
    }

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_NOLOGO = '1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                run 'dotnet restore Homies.sln'
            }
        }

        stage('Build') {
            steps {
                run 'dotnet build Homies.sln --no-restore --configuration Release'
            }
        }

        stage('Test') {
            steps {
                run 'dotnet test Homies.sln --no-build --configuration Release --verbosity normal'
            }
        }
    }

    post {
        success {
            echo 'Build and all tests passed.'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}
