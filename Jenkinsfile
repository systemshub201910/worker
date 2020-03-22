import java.time.*
import java.time.format.DateTimeFormatter

pipeline {
    agent {
        node {
            label 'k8s-master'
        }
    }
    
    stages {
    
        stage ('Building Docker image') {
            steps {
               sh 'docker build -t gcr.io/k8s-dec2-test/worker-app .'
            }
        }
        
        stage ('Setting Google Porject') {
            steps {
                sh 'gcloud config set project k8s-dec2-test'
                sh 'gcloud config list'
            }
        }
        
        stage ('Push Docker image to Google Container Registry') {
            steps {
                sh 'docker push gcr.io/k8s-dec2-test/worker-app'
            }
        }
        
        stage ('Authenticate with Google Kubernetes Cluster') {
            steps {
                sh 'gcloud container clusters get-credentials voting-app --zone asia-south1-a --project k8s-dec2-test'
            }
        }
        
        stage ('Get current K8s cluster context') {
            steps {
                sh "kubectl config current-context"
            }
        }
        
        stage ('Deploy Application') {
            steps {
                sh 'kubectl apply -f ./k8s-specs'
            }
        }
    }
}
