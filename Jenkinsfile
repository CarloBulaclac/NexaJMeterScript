pipeline {
    agent any

    triggers {
        cron('H 2 * * *')
    }

    environment {
        RESULTS = 'results.jtl'
        REPORT_DIR = 'report'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                sh '''
                rm -rf ${RESULTS} ${REPORT_DIR}
                '''
            }
        }

        stage('Run JMeter Test') {
            steps {
                sh '''
                jmeter -n \
                  -t test_plan.jmx \
                  -l ${RESULTS} \
                  -e -o ${REPORT_DIR}
                '''
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'results.jtl, report/**', fingerprint: true
            }
        }
    }
}
