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
                    deploy("dev", 7001)
                }
            }
        }

        stage('tests-on-dev') {
            steps {
                script{
                    test("dev")
                }
            }
        }
        stage('deploy-to-staging') {
            steps {
                script{
                    deploy("stg", 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script{
                    test("stg")
                }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                script{
                    deploy("preprd", 7003)
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script{
                    test("preprd")
                }
            }
        }
        stage('deploy-to-prod') {
            steps {
                script{
                    deploy("prd", 7004)
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script{
                    test("prd")
                }
            }
        }
    }
}



def build(){
    echo "Installing all required depdendencies.."
    //bat "dir C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Jenkins-konvejers"
    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    bat "pip install -r requirements.txt"

}

def deploy(String environment, int port){
    echo "Deployment to ${environment} has started.."
    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    bat "pm2 delete \"greetings-app-${environment}\"" & EXIT /B 0
    bat "pm2 start app.py --name \"greetings-app-${environment}\" -- --port ${port}"
}

def test(String environment){
    echo "Testing ${test_set} test set on ${environment} has started.."
    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/mtararujs/course-js-api-framework.git'
    bat "npm install"
    bat "npm run $npm run greetings greetings_${environment}"
}