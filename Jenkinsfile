pipeline {
    agent any


    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                            sh '''
                                git config user.email anandstyrone0316@gmail.com
                                git config user.name Nandu

                                echo "Contents before update:"
                                cat deployment.yaml

                                sed -i "s+anandnandu0316/test.*+anandnandu0316/test:${DOCKERTAG}+g" deployment.yaml

                                echo "Contents after update:"
                                cat deployment.yaml

                                git add .
                                git commit -m "Done by Jenkins Job changemanifest: ${BUILD_NUMBER}" || echo "No changes to commit"
                                git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main
                            '''
                        }
                    }
                }
            }
        }
    }
}

