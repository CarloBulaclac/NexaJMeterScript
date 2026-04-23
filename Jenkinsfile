pipeline {
agent any

environment {
    JMETER = '/opt/jmeter/bin/jmeter'
    RESULTS = 'results.jtl'
    REPORT_DIR = 'report'
}

stages {

    stage('Install JMeter (if needed)') {
        steps {
            sh '''
            if [ ! -x /opt/jmeter/bin/jmeter ]; then
              echo "JMeter not found. Installing..."

              apt update
              apt install -y wget unzip

              wget -q https://downloads.apache.org/jmeter/binaries/apache-jmeter-5.6.3.tgz
              tar -xzf apache-jmeter-5.6.3.tgz
              mv apache-jmeter-5.6.3 /opt/jmeter

            else
              echo "JMeter already installed. Skipping install."
            fi
            '''
        }
    }

    stage('Run JMeter Test') {
        steps {
            sh '''
            rm -rf ${REPORT_DIR} ${RESULTS}
            ${JMETER} -n \
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
