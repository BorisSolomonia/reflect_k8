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
                git url: 'https://github.com/BorisSolomonia/reflect_k8.git', branch: 'master', credentialsId: "${GIT_CREDENTIALS_ID}"
            }
        }
        stage('Deploy') {
            steps {
                script {
                    step([
                        $class: 'KubernetesEngineBuilder',
                        projectId: env.PROJECT_ID,
                        cluster: env.CLUSTER,  // Corrected cluster field
                        location: env.ZONE,
                        manifestPattern: 'postgres-deployment.yaml',
                        credentialsId: env.GC_KEY,  // Using the correct credentials ID
                        verifyDeployments: true
                    ])
                }
            }
        }
    }
}
