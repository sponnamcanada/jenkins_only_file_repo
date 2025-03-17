pipeline {
    agent any

    // Define parameters for repository and branch
    parameters {
        choice(name: 'REPO_NAME', choices: ['rpa_uipath_test', 'new_jenkins_test', 'uipathjenkins'], description: 'Select the repository to build')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build (default: main)')
    }

    environment {
        // Set versioning components
        MAJOR = '1'  // Major version can be static or dynamic
        MINOR = '0'  // Minor version can be static or dynamic
        BUILD_NUMBER =  "${env.BUILD_NUMBER}"  // Build number is dynamic and provided by Jenkins

        // Orchestrator Services (configure these as per your project setup)
        UIPATH_ORCH_URL = "https://cloud.uipath.com"
        UIPATH_ORCH_LOGICAL_NAME = "cloud_siva_ponnam"
        UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
        UIPATH_ORCH_FOLDER_NAME = "jenkins uipath"
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
                    echo "Building the selected repository:  ${repoName}"
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
                    outputPath: "${WORKSPACE}/Output/${BUILD_NUMBER}",  // Use BUILD_NUMBER as output directory
                    projectJsonPath: "S:\\jenkins workspace\\workspace\\new_jenkins_test\\project.json",  // Path to the project.json file
                    version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${BUILD_NUMBER}"],  // Dynamic versioning
                    useOrchestrator: false,  // Set to true if you want to use Orchestrator
                    credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey'],  // Provide API credentials
                    traceLevel: 'None'  // Trace level can be adjusted as per your requirement
                )
                 //echo "${projectJsonPath}"
                echo "UiPath Pack completed"
            }
            
        }

        stage('Deploy') {
            steps {
                echo "entered to deployment after completing the creation of nupkg"
                echo "${WORKSPACE}\\Output\\${BUILD_NUMBER}\\new_jenkins_test.${MAJOR}.${MINOR}.${BUILD_NUMBER}.nupkg"

                // UiPathDeploy step to deploy the package to UiPath Orchestrator
                UiPathDeploy(
                    traceLevel: 'Verbose',
                    packagePath: "${WORKSPACE}/Output/${BUILD_NUMBER}/new_jenkins_test.${MAJOR}.${MINOR}.${BUILD_NUMBER}.nupkg",  // Use the dynamically generated package path
                    orchestratorAddress: "${UIPATH_ORCH_URL}",  // Orchestrator URL
                    orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", // Your Orchestrator Tenant Name
                    folderName: "${UIPATH_ORCH_FOLDER_NAME}",  // Folder name
                    createProcess: false,
                    environments:'',
                    entryPointPaths: '',
                    credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']  // Credentials
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
