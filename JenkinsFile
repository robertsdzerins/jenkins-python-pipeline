pipeline {
    agent any

    stages {
        stage('install-pip-deps') {
            steps {
                script {
                    installDeps()
                }
            }
        }

        stage('deploy-to-dev') { steps { script { deploy('dev', 7001) }}}
        stage('tests-on-dev') { steps { script { runTests('dev') }}}

        stage('deploy-to-staging') { steps { script { deploy('staging', 7002) }}}
        stage('tests-on-staging') { steps { script { runTests('staging') }}}

        stage('deploy-to-preprod') { steps { script { deploy('preprod', 7003) }}}
        stage('tests-on-preprod') { steps { script { runTests('preprod') }}}

        stage('deploy-to-prod') { steps { script { deploy('prod', 7004) }}}
        stage('tests-on-prod') { steps { script { runTests('prod') }}}
    }
}

def installDeps() {
    echo "Installing Python dependencies..."
    sh '''
        git clone https://github.com/mtararujs/python-greetings
        cd python-greetings
        ls
        pip3 install -r requirements.txt
    '''
}

def deploy(env, port) {
    echo "Deploying to ${env} environment on port ${port}..."
    sh """
        git clone https://github.com/mtararujs/python-greetings app-${env}
        pm2 delete greetings-app-${env} || true
        cd app-${env}
        pm2 start app.py --name greetings-app-${env} -- --port ${port}
    """
}

def runTests(env) {
    echo "Running tests on ${env} environment..."
    sh """
        git clone https://github.com/mtararujs/course-js-api-framework tests-${env}
        cd tests-${env}
        npm install
        npm run greetings greetings_${env}
    """
}

