pipeline {
    agent any 
    

    environment {
        // 🔧 Update paths as per your Jenkins agent setup
        JMETER_HOME = "C:\\apache-jmeter-5.6.3"
        TEST_PLAN   = "HR_ThreadGroup.jmx"       // path inside your GitHub repo
        RESULT_JTL  = "D:\\Workspace\\JMeter\\2\\results.jtl"
        REPORT_DIR  = "D:\\Workspace\\JMeter\\2\\report"
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                // 🧩 Pull your JMeter test plan from GitHub
                // Replace URL and credentialsId with your values
                git(
                    branch: 'main',
                    url: 'https://github.com/minmax99/myjenkinspipeline.git',
                    credentialsId: '' //'github-credentials-id'   // Jenkins credential ID for GitHub
                )
            }
        }

        stage('Run JMeter Test') {
            steps {
                script {

                    // 🧪 Execute JMeter in non-GUI mode
                    bat """
                        "%JMETER_HOME%\\bin\\jmeter.bat" ^
                        -n -t "%WORKSPACE%\\${TEST_PLAN}" ^
                        -l "%RESULT_JTL%" ^
                        -e -o "%REPORT_DIR%"
                    """
                }
            }
        }

        stage('Publish JMeter Report') {
            steps {
                // 📊 Publish HTML dashboard in Jenkins
                publishHTML([[
                    reportName : 'JMeter Test Report',
                    reportDir  : "${env.REPORT_DIR}",
                    reportFiles: 'index.html',
                    keepAll    : true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: false
                ]])
            }
        }
    }

    post {
        always {
            echo "Archiving JMeter test results..."
            archiveArtifacts artifacts: 'results/**/*.jtl', allowEmptyArchive: true
        }
        failure {
            echo "❌ JMeter tests failed. Please review the logs and HTML report."
        }
    }
}
