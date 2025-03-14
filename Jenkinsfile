pipeline {
    agent any
    
    parameters {
        choice(name: 'REPO_TO_BUILD', choices: ['rpa_uipath_test', 'new_jenkins_test', 'uipathjenkins'], description: 'Select which repository to build')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build (default: main)')
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Check out the selected repository
                    if (params.REPO_TO_BUILD == 'rpa_uipath_test') {
                        echo "Building rpa_uipath_test on branch ${params.BRANCH}"
                        git url: 'https://github.com/sponnamcanada/rpa_uipath_test.git', branch: params.BRANCH
                    } else if (params.REPO_TO_BUILD == 'new_jenkins_test') {
                        echo "Building new_jenkins_test on branch ${params.BRANCH}"
                        git url: 'https://github.com/sponnamcanada/new_jenkins_test.git', branch: params.BRANCH
                    } else if (params.REPO_TO_BUILD == 'uipathjenkins') {
                        echo "Building uipathjenkins on branch ${params.BRANCH}"
                        git url: 'https://github.com/sponnamcanada/uipathjenkins.git', branch: params.BRANCH
                    } else {
                        error "Invalid repository selected"
                    }
                }
            }
        }
        
        stage('Build') {
            steps {
                echo "Building the selected repository"
                // Add your build steps here (e.g., compile, test, etc.)
            }
        }
        
        stage('Post-build') {
            steps {
                echo "Performing post-build actions"
                // Add any post-build steps, like notifications or artifact archiving
            }
        }
    }
}
