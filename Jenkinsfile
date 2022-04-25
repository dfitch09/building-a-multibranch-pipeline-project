pipeline {
    agent any
    parameters {
        string(name: 'APP', defaultValue: 'cm-ui', description: 'App Name')
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello world!"'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"

	             script {
                        GIT_COMMIT_HASH = sh (script: "git rev-parse --short HEAD", returnStdout: true)
                        sh "echo $GIT_COMMIT_HASH"
                        sh "echo ${APP}"
                        
                    }
            }
        }
    }
}
