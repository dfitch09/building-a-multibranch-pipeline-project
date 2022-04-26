pipeline {
   agent any
    parameters {
        string(name: 'APP', defaultValue: 'cm-ui', description: 'App Name')
        string(name: 'REGISTRY', defaultValue: 'harbor.best-im.com', description: 'Harbor REGISTRY URI')
        // NOT USED: string(name: 'build', defaultValue: 'Dockerfile', description: 'Docker Build File')
    }
      stages {
            stage('Clean, Pull, and Hash') {
                steps {
                    script {
                        cleanWs()
                    }
                        git branch: "$GIT_BRANCH",
                        ////credentialsId: 'GITHUB_TOKEN',
                        url: 'https://github.com/dfitch09/building-a-multibranch-pipeline-project'
                        ////url: 'https://github.ibm.com/FDA-BEST/case_mngt_ui.git'
                    script {
                        GIT_COMMIT_HASH = sh (script: "git rev-parse --short HEAD", returnStdout: true)
                        sh "echo ${params.APP}"
                        sh "echo $GIT_COMMIT_HASH"
                    }
                }
            }

            stage('Sonarqube Code Analysis') {
                 steps {
                     script {
                      def scannerHome = tool 'sonarqube';
                      withSonarQubeEnv('sonarqube') {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=${params.APP}"
                      }
                     }
                 }
            }
            
            stage('Build') {
                steps {
                    sh "echo 'Build...'"
                ////withVault(configuration: [timeout: 60, vaultCredentialId: 'vault-role', vaultUrl: 'https://nonprod-vault.best-im.com'], vaultSecrets: [[path: '$GIT_BRANCH/cm-ui', secretValues: [[vaultKey: 'REACT_APP_NODE_SERVER'], [vaultKey: 'REACT_APP_CHART_REVIEW'], [vaultKey: 'REACT_APP_KEYCLOAK_URL'], [vaultKey: 'REACT_APP_KEYCLOAK_REALM'], [vaultKey: 'REACT_APP_KEYCLOAK_CLIENT'], [vaultKey: 'REACT_APP_KEYCLOAK_CR_ADMIN_GRP'], [vaultKey: 'REACT_APP_SUPERSET_PORT']]]]) {
                    //sh "echo ${REACT_APP_NODE_SERVER}" // echo 1 setting...
                }

                /*
                sh """docker build -f $GIT_BUILD . -t ${params.REGISTRY}/$GIT_BRANCH/${params.APP}:latest \
                  --build-arg REACT_APP_NODE_SERVER=$REACT_APP_NODE_SERVER \
                  --build-arg REACT_APP_CHART_REVIEW=$REACT_APP_CHART_REVIEW \
                  --build-arg REACT_APP_KEYCLOAK_URL=$REACT_APP_KEYCLOAK_URL \
                  --build-arg REACT_APP_KEYCLOAK_REALM=$REACT_APP_KEYCLOAK_REALM \
                  --build-arg REACT_APP_KEYCLOAK_CLIENT=$REACT_APP_KEYCLOAK_CLIENT \
                  --build-arg REACT_APP_KEYCLOAK_CR_ADMIN_GRP=$REACT_APP_KEYCLOAK_CR_ADMIN_GRP \
                  --build-arg REACT_APP_SUPERSET_PORT=$REACT_APP_SUPERSET_PORT """                  
                  }
                  
                }
                */
            }
            
            stage('Image Push') {
                steps {
                    sh "echo 'Image Push...'"
                    sh "echo ${params.REGISTRY}/$GIT_BRANCH/${params.APP}"
                  ////sh "docker push ${params.REGISTRY}/$GIT_BRANCH/${params.APP}:latest"
                  ////sh "docker tag ${params.REGISTRY}/$GIT_BRANCH/${params.APP}:latest ${params.REGISTRY}/$BRANCH/${params.APP}:$GIT_COMMIT_HASH"
                  ////sh "docker push ${params.REGISTRY}/$GIT_BRANCH/${params.APP}:$GIT_COMMIT_HASH"
                }
            }
            
            stage('Build Cleanup') {
                steps {
                  sh "echo 'Build Cleanup'"
                  sh "echo ${params.REGISTRY}/$GIT_BRANCH/${params.APP}:latest"
                  ////sh "docker rmi ${params.REGISTRY}/$GIT_BRANCH/${params.APP}:latest"
                  cleanWs()
                }
            }
            
            stage('Manual Git of Gitops AWS') {
                steps {
                    sh "echo 'Manual Git of Gitops AWS'"
                }
                /*
                steps {
                    withCredentials([usernamePassword(credentialsId: 'GITOPS', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                      sh ('git clone https://$GIT_USERNAME:$GIT_PASSWORD@github.ibm.com/FDA-BEST/gitops_aws.git')
                        dir("./gitops_aws/apps/${params.APP}/overlays/$GIT_BRANCH/") {
                        sh "kustomize edit set image ${params.REGISTRY}/$GIT_BRANCH/${params.APP}:$GIT_COMMIT_HASH"
                        sh "git config --global user.email 'yunnwei.swei@ibm.com'"
                        sh "git config --global user.name 'Yunnwei Swei'"
                        sh "git commit -am 'Update $BRANCH ${params.APP}' --allow-empty"
                        }
                    }
                }
                */
            }
            stage('Commit to Gitops Repo') {
                steps {
                    sh "echo 'Commit to Gitops Repo'"
                }
                /*
                steps {
                    dir("./gitops_aws/apps/${params.APP}/overlays/$BRANCH") {
                        withCredentials([usernamePassword(credentialsId: 'GITOPS', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                          sh ('git push --set-upstream https://$GIT_USERNAME:$GIT_PASSWORD@github.ibm.com/FDA-BEST/gitops_aws.git master')
                        }
                    }
                }
                */
            }       
        }
}
