pipeline {
    agent any
    stages {
        stage('Checkout and Build') {
            steps {
                   echo "checking out the main"
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
