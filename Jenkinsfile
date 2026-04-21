pipeline {
    agent any

    triggers {
        cron('H 2 * * *') // daily run
    }

    environment {
        JMETER_HOME = '/opt/jmeter'
        TEST_PLAN = 'test_plan.jmx'
        RESULTS = 'results.jtl'
        REPORT_DIR = 'report'
    }

    stages {
        stage('Checkout') {
            steps {
                // Jenkins already checks out SCM, but this is safe
                git 'https://github.com/CarloBulaclac/NexaJMeterScript.git'
            }
        }

        stage('Run JMeter Test') {
            steps {
                sh '''
                rm -rf ${REPORT_DIR}

                ${JMETER_HOME}/bin/jmeter -n \
                  -t ${TEST_PLAN} \
                  -l ${RESULTS} \
                  -e -o ${REPORT_DIR}
                '''
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: '*.jtl, report/**', fingerprint: true
            }
        }
    }
}
