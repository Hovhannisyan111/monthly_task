@Library('monthly_task_library') _

pipeline {
    agent any
    environment {
        MONTHLY_DAY        = '27'
        RETRY_INTERVAL_MIN = '5'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running Build...'
                //error 'Simulated Build failure'
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
                    return monthlyPipeline.isMonthlyTaskDue(this)
                }
            }
            steps {
                echo 'Running monthly task (sending artifacts to S3)...'
                // sh 'aws s3 sync ./dist s3://your-bucket/artifacts'
            }
        }
    }
    post {
        success {
            script {
                monthlyPipeline.clearAll(this)
            }
        }
        failure {
            script {
                monthlyPipeline.handleFailure(this)
            }
        }
        always {
            echo "Build finished with: ${currentBuild.currentResult}"
        }
    }
}
