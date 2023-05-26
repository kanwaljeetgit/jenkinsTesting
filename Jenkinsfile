pipeline {
    agent any
    stages {
        stage('Checkout and Build') {
            steps {
                   echo "checking out the "BRNACH_NAME
            }
        }
    }

    post {
        always {
            // clean up workspace
            deleteDir()

            echo 'Workspace cleaned up.'
        }
    }
}
