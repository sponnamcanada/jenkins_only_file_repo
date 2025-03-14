pipeline {
    agent any

    // Define parameters for repository and branch
    parameters {
        choice(name: 'REPO_NAME', choices: ['rpa_uipath_test', 'new_jenkins_test', 'uipathjenkins'], description: 'Select the repository to build')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build (default: main)')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Define the repository URLs here (outside the environment block)
                    def REPO_URLS = [
                        'rpa_uipath_test': 'https://github.com/sponnamcanada/rpa_uipath_test.git',
                        'new_jenkins_test': 'https://github.com/sponnamcanada/new_jenkins_test.git',
                        'uipathjenkins': 'https://github.com/sponnamcanada/uipathjenkins.git'
                    ]

                    // Get the selected repository name from the parameter
                    def repoName = params.REPO_NAME
                    echo "Repository selected: ${repoName}"

                    // Check if the repository is one of the valid choices and perform the checkout
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

                // Add the echo for the workspace path
                echo "Building with workspace: ${WORKSPACE}"

                // UiPathPack build step
                UiPathPack(
                    outputPath: "${WORKSPACE}/Output/${env.BUILD_NUMBER}",
                    projectJsonPath: "project.json",
                    version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
                    useOrchestrator: false,
                    credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey'],
                    traceLevel: 'None'
                )
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
