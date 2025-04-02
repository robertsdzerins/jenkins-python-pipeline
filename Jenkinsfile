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
    bat '''
        git clone https://github.com/mtararujs/python-greetings
        cd python-greetings
        dir
        "C:\\Users\\User\\AppData\\Local\\Programs\\Python\\Python311\\Scripts\\pip.exe" install -r requirements.txt
    '''
}

def deploy(env, port) {
    echo "Deploying to ${env} environment on port ${port}..."
    bat """
        git clone https://github.com/mtararujs/python-greetings app-${env}
        cd app-${env} && pm2 delete greetings-app-${env} || exit 0
        cd app-${env} && pm2 start app.py --name greetings-app-${env} -- --port ${port}
    """
}

def runTests(env) {
    echo "Running tests on ${env} environment..."
    bat """
        git clone https://github.com/mtararujs/course-js-api-framework tests-${env}
        cd tests-${env} && npm install
        cd tests-${env} && npm run greetings greetings_${env}
    """
}
