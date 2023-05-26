pipeline {
    agent any

    parameters {
        string(name: 'branch_name', defaultValue: '', description: 'Name of the new branch to create')
        multiBranchParam(name: 'repos_to_run', description: 'Select the repositories to run the job upon', filterable: true) {
            branchParam(name: 'run_first_repo', projectBranch: '<FIRST_REPOSITORY_URL>', filterable: true)
            branchParam(name: 'run_second_repo', projectBranch: '<SECOND_REPOSITORY_URL>', filterable: true)
        }
    }

    stages {
        stage('Checkout and Build') {
            steps {
                // check which repositories to run the job upon
                def run_first_repo = params.repos_to_run.contains('run_first_repo')
                def run_second_repo = params.repos_to_run.contains('run_second_repo')

                // clone and checkout the develop branch for the first repository, if selected
                if (run_first_repo) {
                    git url: '<FIRST_REPOSITORY_URL>'
                    checkout branch: 'develop'

                    // pull the latest changes from the develop branch
                    git pull

                    echo 'Cloned and pulled code from first repository.'

                    // create a new branch with the name provided by the user
                    git branch "${params.branch_name}"
                    git checkout "${params.branch_name}"

                    // update the version in the pom file
                    sh "mvn versions:set -DnewVersion='${params.branch_name}-SNAPSHOT'"

                    // commit the changes and push them to the new branch
                    git commit -am "Updated version in pom file"
                    git push origin "${params.branch_name}"

                    echo 'Created and pushed code to new branch in first repository.'

                    // switch back to the develop branch and pull the latest changes
                    git checkout develop
                    git pull

                    echo 'Switched to develop branch in first repository.'
                }

                // clone and checkout the develop branch for the second repository, if selected
                if (run_second_repo) {
                    dir('<SECOND_REPOSITORY_DIRECTORY>') {
                        git url: '<SECOND_REPOSITORY_URL>'
                        checkout branch: 'develop'

                        // pull the latest changes from the develop branch
                        git pull

                        echo 'Cloned and pulled code from second repository.'

                        // create a new branch with the name provided by the user
                        git branch "${params.branch_name}"
                        git checkout "${params.branch_name}"

                        // update the version in the pom file
                        sh "mvn versions:set -DnewVersion='${params.branch_name}-SNAPSHOT'"

                        // commit the changes and push them to the new branch
                        git commit -am "Updated version in pom file"
                        git push origin "${params.branch_name}"

                        echo 'Created and pushed code to new branch in second repository.'

                        // switch back to the develop branch and pull the latest changes
                        git checkout develop
                        git pull

                        echo 'Switched to develop branch in second repository.'
                    }
                }

                // update the version in the pom file for the first repository, if selected
                if (run_first_repo) {
                    git checkout "${params.branch_name}"
                    sh "mvn versions:set -DnewVersion='${params.branch_name}-SNAPSHOT'"
                    git commit -am "Updated version in pom file"
                    git push origin "${params.branch_name}"

                    echo 'Updated version in pom file and pushed changes to new branch in first repository.'
                }

                // update the version in the pom file for the second repository, if selected
                if (run_second_repo) {
                    dir('<SECOND_REPOSITORY_DIRECTORY>') {
                        git checkout "${params.branch_name}"
                        sh "mvn versions:set -DnewVersion='${params.branch_name}-SNAPSHOT'
                        git commit -am "Updated version in pom file"
                        git push origin "${params.branch_name}"

                        echo 'Updated version in pom file and pushed changes to new branch in second repository.'
                    }
                }

                // build and deploy the code for the first repository, if selected
                if (run_first_repo) {
                    dir('<FIRST_REPOSITORY_DIRECTORY>') {
                        git checkout "${params.branch_name}"

                        // build and deploy the code
                        sh 'mvn clean install'
                        sh 'aws deploy'

                        echo 'Built and deployed code for first repository.'
                    }
                }

                // build and deploy the code for the second repository, if selected
                if (run_second_repo) {
                    dir('<SECOND_REPOSITORY_DIRECTORY>') {
                        git checkout "${params.branch_name}"

                        // build and deploy the code
                        sh 'mvn clean install'
                        sh 'aws deploy'

                        echo 'Built and deployed code for second repository.'
                    }
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
