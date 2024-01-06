pipeline {
  agent {
    kubernetes {
      label 'sample-app'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  containers:
  - name: gcloud-kubectl-docker
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
  }
  stages {
    stage('Build and push image with Container Builder') {
      steps {
        container('gcloud-kubectl-docker') {
          sh "gcloud auth activate-service-account --key-file=disco-extended-arcana-408715-08a0471e7544.json"
          sh "gcloud config set project extended-arcana-408715"
          sh "PYTHONUNBUFFERED=1 gcloud builds submit -t gcr.io/extended-arcana-408715/adservice ."
        }
      }
   }
    stage('deployment'){
      steps{
        container('gcloud-kubectl-docker'){
          sh"gcloud container clusters get-credentials cluster-2 --zone us-east1-b --project extended-arcana-408715"
          sh"kubectl apply -f adservice.yaml"
}
}

      }
    }
  }
