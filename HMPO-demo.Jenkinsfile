pipeline {
    agent any
    environment {
        GLOBAL_VARIABLE = "SOME_GLOBAL_VARIABLE"
    }
    parameters {
        string(name: "BUILD_NR", defaultValue: '', description: 'Give your Artifacts a Build number')
        booleanParam(name: 'BUILD_ARTIFACTS_ONLY', defaultValue: false, description: '')
        booleanParam(name: 'DEPLOY_ARTIFACTS_ONLY', defaultValue: false, description: '')
        booleanParam(name: 'BUILD_AND_DEPLOY', defaultValue: false, description: '')
        choice(name: 'ENVIRONMENT', choices: ['DEVELOPMENT', 'REGRESSION', 'STAGING', 'PRODUCTION'], description: 'Please choose the environment you wish to deploy to')
    }
    stages {
        stage ('BUILD ARTIFACTS') {
            when { 
                not 
                    { environment name: "ACTION", value: "DEPLOY_ARTIFACTS_ONLY" }
            }
            steps {
                BuildArtifacts("${BUILD_NR}")
            }
        }
        stage ('UNIT TESTS') {
            steps {
                UnitTests()
            }
        }
        stage ('INTEGRATION TESTS') {
            steps {
                IntegrationTests()
            }
        }
        stage ('DEPLOY ARTIFACTS') {
            parallel {
                stage ('DEPLOY TO SERVER-1') {
                    agent any
                    environment {
                        LOCAL_VARIABLE = "SOME_LOCAL_VARIABLE-1"
                    }
                    when { 
                        anyOf { 
                            environment name: "DEPLOY_ARTIFACTS_ONLY", value: "true" 
                            environment name: "BUILD_AND_DEPLOY", value: "true" 
                        }
                    }
                    steps {
                        DeployArtifacts("${BUILD_NR}", "${ENVIRONMENT}")
                    }
                }
                stage ('DEPLOY TO SERVER-2') {
                    agent any
                    environment {
                        LOCAL_VARIABLE = "SOME_LOCAL_VARIABL-2"
                    }
                    when { 
                        anyOf { 
                            environment name: "DEPLOY_ARTIFACTS_ONLY", value: "true" 
                            environment name: "BUILD_AND_DEPLOY", value: "true" 
                        }
                    }
                    steps {
                        DeployArtifacts("${BUILD_NR}", "${ENVIRONMENT}")
                    }
                }
            }
        }
    }
}

def BuildArtifacts(buildNr) {
    sh '''
        echo "Building some artifacts with build number: \$buildNr and pushing to repository!"
        sleep 5s
    '''
}

def UnitTests() {
    sh '''
        echo "running some Unit Tests!"
        sleep 5s
    '''
}

def IntegrationTests() {
    sh '''
        echo "running some Integration Tests!"
        sleep 5s
    '''
}

def DeployArtifacts(buildNr, environment) {
    sh '''
        echo "Deploying \$buildNr to Enrionment: \$environment"
        sleep 5s
    '''
}
