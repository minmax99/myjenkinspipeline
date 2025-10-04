pipeline {
    agent any

    environment {
        JMETER_HOME = "C:\\apache-jmeter-5.6.3"   // adjust to your JMeter path
        TEST_PLAN  = "D:\\Workspace\\JMeter\\HR_ThreadGroup.jmx"
        RESULT_JTL = "D:\\Workspace\\JMeter\\2\\results.jtl"
        REPORT_DIR = "D:\\Workspace\\JMeter\\2"
    }

    stages {
        stage('Run JMeter Test') {
            steps {
                script {
                    bat """
                        "%JMETER_HOME%\\bin\\jmeter.bat" -n -t "%TEST_PLAN%" -l "%RESULT_JTL%" -e -o "%REPORT_DIR%"
                    """
                }
            }
        }

        stage('Publish JMeter Report') {
            steps {
                publishHTML([[
                    reportName : 'JMeter Report',
                    reportDir  : "${env.REPORT_DIR}",
                    reportFiles: 'index.html',
                    keepAll    : true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: true
                ]])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/*.jtl, **/*.log', allowEmptyArchive: true
        }
    }
}
