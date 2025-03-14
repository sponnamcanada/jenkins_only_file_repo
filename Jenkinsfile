pipeline {
    agent any

    environment {
        // Define the repository URLs to automatically map the repository name
        REPO_URLS = [
            'rpa_uipath_test': 'https://github.com/sponnamcanada/rpa_uipath_test.git',
            'new_jenkins_test': 'https://github.com/sponnamcanada/new_jenkins_test.git',
            'uipathjenkins': 'https://github.com/sponnamcanada/uipathjenkins.git'
        ]
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Get the repository name from the webhook event
                    def repoName = env.GIT_URL?.split('/')[-1]?.replace('.git', '')

                    echo "Repository detected: ${repoName}"

                    if (repoName && REPO_URLS.containsKey(repoName)) {
                        echo "Building ${repoName} on branch ${params.BRANCH}"
                        git url: REPO_URLS[repoName], branch: params.BRANCH
                    } else {
                        error "Repository ${repoName} not recognized"
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
