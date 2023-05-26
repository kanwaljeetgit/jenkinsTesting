pipeline {
    agent any
    stages {
        stage('Checkout and Build') {
            steps {
                step{
                   echo "checking out the "BRNACH_NAME
                }
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
