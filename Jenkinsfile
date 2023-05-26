pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'new-branch', description: 'Name of the branch to create')
        string(name: 'POM_VERSION', defaultValue: '1.0.0', description: 'Version to update in pom.xml')
        string(name: 'REPOSITORY_URL', defaultValue: '1.0.0', description: 'Version to update in pom.xml')
    }

    stages {
        stage('Create Branch') {
            steps {
                script {
                    def branchName = params.BRANCH_NAME
                    git url: params.REPOSITORY_URL
                    checkout branch: branchName
                    git branch: branchName, changelog: false, poll: false
                    "$pwd"
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }

    post {
        always {
            deleteDir()
        }
    }
}
