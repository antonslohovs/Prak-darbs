pipeline {
    agent any
    triggers{ pollSCM('*/1 * * * *') }
    

    stages {
        stage('install-pip-deps') {
            steps {
                script{
                    build()
                }
            }
        }
        stage('deploy-to-dev') {
            steps {
                script{
                    deploy("DEV", 7001)
                }
            }
        }

        stage('tests-on-dev') {
            steps {
                script{
                    test("BOOKS", "DEV")
                }
            }
        }
        stage('deploy-to-staging') {
            steps {
                script{
                    deploy("STG", 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script{
                    test("BOOKS", "STG")
                }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                script{
                    deploy("PREPRD", 7003)
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script{
                    test("BOOKS", "PRD")
                }
            }
        }
        stage('deploy-to-prod') {
            steps {
                script{
                    deploy("PRD", 7004)
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script{
                    test("BOOKS", "PRD")
                }
            }
        }
    }
}

// for windows: bat "npm.."
// for linux/macos: sh "npm .."

def build(){
    echo "Installing all required depdendencies.."
    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/mtararujs/python-greetings'
    bat "ls"
  //  bat "npm install -g pm2"
}

def deploy(String environment, int port){
    echo "Deployment to ${environment} has started.."
    bat "pm2 delete \"greetings-app-${environment}\"" & set errorlevel=0
    bat "pm2 start -n \"books-${environment}\" index.js -- -- ${port}"
}

def test(String test_set, String environment){
    echo "Testing ${test_set} test set on ${environment} has started.."
    git branch: 'main', poll: false, url: 'https://github.com/antonslohovs/course-js-api-framework_2.git'
    bat "npm run ${test_set} ${test_set}_${environment}"
}