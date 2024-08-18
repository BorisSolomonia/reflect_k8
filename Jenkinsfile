pipeline {
    agent any
    environment {
        GIT_CREDENTIALS_ID = 'github-credentials-id'
        GC_KEY = 'gke-credentials-id'
        REGISTRY_URI = 'asia-south1-docker.pkg.dev'
        PROJECT_ID = 'reflection01-431417'
        ARTIFACT_REGISTRY = 'reflection-artifacts'
        CLUSTER = 'reflection-cluster-1'
        ZONE = 'us-central1'  // Ensure this matches the zone where your cluster is located
        REPO_URL = "${REGISTRY_URI}/${PROJECT_ID}/${ARTIFACT_REGISTRY}"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/BorisSolomonia/reflection-auth-server.git', branch: 'master', credentialsId: "${GIT_CREDENTIALS_ID}"
            }
        }
        stage('Deploy') {
            step([
                $class: 'KubernetesEngineBuilder',
                projectId: env.PROJECT_ID,
                cluster: "${env.CLUSTER} (${env.ZONE})", // Ensure this is correct
                location: env.ZONE,
                manifestPattern: 'auth-server-deployment.yaml',
                credentialsId: "${PROJECT_ID}",
                verifyDeployments: true
                ])
        }
    }
}
