pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello world!"'

	             script {
                        GIT_COMMIT_HASH = sh (script: "git rev-parse --short HEAD", returnStdout: true)
                        sh "echo $GIT_COMMIT_HASH"
                        sh "echo 'cm-ui'"
                        sh "echo ${GIT_BRANCH}"
                    }
            }
        }
    }
}
