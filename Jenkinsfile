pipeline {

    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        CI = 'true'
    }
/* 
    triggers {

        cron('''
            TZ=Asia/Kolkata
            */3 * * * *
        ''')

    }
 */
    stages {

        stage('Checkout Code') {

            steps {

                echo 'Getting code from GitHub...'

                checkout scm

            }

        }


        stage('Check Node Version') {

            steps {

                bat 'node -v'

                bat 'npm -v'

            }

        }


        stage('Install Dependencies') {

            steps {

                echo 'Installing project dependencies...'

                bat 'npm ci'

            }

        }


        stage('Install Playwright Browser') {

            steps {

                echo 'Installing Chromium browser...'

                bat 'npx playwright install chromium'

            }

        }


        stage('Run Playwright Tests') {

            steps {

                echo 'Running Playwright tests...'

                bat 'npx playwright test --project=chromium'

            }

        }

    }


    post {

        always {

            echo 'Publishing Playwright report...'

            publishHTML([

                allowMissing: true,

                alwaysLinkToLastBuild: true,

                keepAll: true,

                reportDir: 'playwright-report',

                reportFiles: 'index.html',

                reportName: 'Playwright HTML Report'

            ])


            archiveArtifacts(

                artifacts: 'test-results/**/*',

                allowEmptyArchive: true

            )

        }


        success {

            echo 'BUILD SUCCESS'

            echo 'All Playwright tests passed.'

        }


        failure {

            echo 'BUILD FAILED'

            echo 'Check Playwright HTML report.'

        }

    }

}