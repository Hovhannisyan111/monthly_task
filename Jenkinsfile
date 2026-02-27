@Library('monthly_task_library') _

pipeline {
    agent any
    environment {
        MONTHLY_DAY        = '28'
        RETRY_INTERVAL_MIN = '2'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running Build...'
                // error 'Simulated Build failure'
            }
        }
        stage('Test') {
            steps {
                echo 'Running Tests...'
                // error 'Simulated Test failure'
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging artefacts...'
                // error 'Simulated Package failure'
            }
        }
        stage('Monthly Task') {
            when {
                expression {
                    return monthlyPipeline.init(this).isMonthlyTaskDue()
                }
            }
            steps {
                script {
                    echo 'Running monthly task (sending artifacts to S3)...'
                }
            }
        }
    }
    post {
        success {
            script {
                monthlyPipeline.init(this).clearAll()
            }
        }
        failure {
            script {
                monthlyPipeline.init(this).handleFailure()
            }
        }
        always {
            echo "Build finished with: ${currentBuild.currentResult}"
        }
    }
}
