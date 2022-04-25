pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"

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
