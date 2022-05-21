def branchEnv = ''
def pathEnv = ''
def pathDev = ''
def pathQA = ''
def pathMaster = ''
def repoName = ''
def gitUrl = ''

pipeline {
    agent any

    environment {
        BRANCH_NAME = "${GIT_BRANCH}"
      	GIT_URL = "${GIT_URL}"
    }

    stages {
        stage('Show Repo') {
            steps {
                script {
                    branchEnv = BRANCH_NAME
                    gitUrl = GIT_URL
                  	repoName = gitUrl.tokenize('/').last().split("\\.")[0]
                }
              	echo "Running Repo URL ${gitUrl}"
              	echo "Running Repo Name${repoName}"
            }
        }
        stage("Creating volumen and network") {
            steps {
                echo "Creating volumen and network ${repoName}"
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh "docker volume create ${repoName}-vol"
                    sh "docker network create -d overlay ${repoName}-nw"
                }
            }
        }
        stage("Removing Docker Stack") {
            steps {
                echo "Removing Docker Stack ${repoName}"
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh "docker stack rm ${repoName}"
                }
            }
        }
        stage("Deploy Docker Stack") {
            steps {
                echo "Running Deploy Docker Stack ${repoName}"
                sh "docker stack deploy -c stack.yaml ${repoName}"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}