pipeline {
    agent { docker { image 'golang' } }
    stages {
        stage('build') {
            steps {
                sh 'go version'
            }
        }
        stage('shiftleft ng scan') {
                sh 'curl https://cdn.shiftleft.io/download/sl > $HOME/sl && chmod a+rx $HOME/sl'
                sh '$HOME/sl auth --token $SL_AUTH_TOKEN'
            }
        }
    }
}