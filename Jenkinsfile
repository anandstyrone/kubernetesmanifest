properties([
    parameters([
        string(name: 'DOCKERTAG', defaultValue: '', description: 'Docker image tag to deploy')
    ])
])

pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-credentials'
        REPO_URL = 'https://github.com/anandstyrone/kubernetesmanifest.git'
        BRANCH = 'main'
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
                        sed -i 's+anandnandu0316/test:[^ ]*+anandnandu0316/test:${params.DOCKERTAG}+g' deployment.yaml
                    """
                    sh """
                        git config user.email "anandstyrone0316@gmail.com"
                        git config user.name "Nandu"
                        git add deployment.yaml
                        git commit -m "Update image tag to ${params.DOCKERTAG} by Jenkins job"
                        git push origin ${env.BRANCH}
                    """
                }
            }
        }

        stage('Trigger Argo CD Sync') {
            steps {
                // Option 1: If you have argocd CLI installed on Jenkins agent
                // sh 'argocd app sync <your-argocd-app-name>'

                // Option 2: If Argo CD is configured to auto-sync on Git changes, skip this stage

                echo "Argo CD application will sync changes automatically or sync manually if configured."
            }
        }
    }
}

