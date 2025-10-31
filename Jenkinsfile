pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-credentials'
        REPO_URL = 'https://github.com/anandstyrone/kubernetesmanifest.git'
        BRANCH = 'main'
    }

    parameters {
        string(name: 'DOCKERTAG', defaultValue: '', description: 'Docker image tag to deploy')
    }

    stages {
        stage('Clone Manifest Repo') {
            steps {
                git url: "${env.REPO_URL}", branch: "${env.BRANCH}", credentialsId: "${env.GIT_CREDENTIALS_ID}"
            }
        }

        stage('Update Kubernetes Manifest') {
            steps {
                script {
                    sh """
                        sed -i 's+raj80dockerid/test:[^ ]*+raj80dockerid/test:${params.DOCKERTAG}+g' deployment.yaml
                    """
                }
            }
        }

        stage('Commit and Push Changes') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.GIT_CREDENTIALS_ID, usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh """
                            git config user.email "anandstyrone0316@gmail.com"
                            git config user.name "Nandu"
                            git add deployment.yaml
                            git commit -m "Update image tag to ${params.DOCKERTAG} by Jenkins job ${env.BUILD_NUMBER}"
                            git remote set-url origin https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git
                            git push origin HEAD:${env.BRANCH}
                        """
                    }
                }
            }
        }

        stage('Trigger Argo CD Sync') {
            steps {
                echo "Argo CD application will sync changes automatically or sync manually if configured."
                // sh 'argocd app sync <your-argocd-app-name>'  // Uncomment if argocd CLI installed & configured
            }
        }
    }
}

