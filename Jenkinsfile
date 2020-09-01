pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  name: elf-pod
  labels:
    service_name: elf-service
    service_type: REST
spec:
  containers:
  - name: helm
    image: bryandollery/terraform-packer-aws-alpine
    command:
    - cat
    tty: true
"""
    }
  }
  environment {
    DOCKER_NAMESPACE = 'lordblackfish'
    SERVICE_NAME = 'mother-service'
  }
  stages {
      stage("Build") {
          steps {
              container('helm') {
                  sh """
		      kubectl apply -f namespace.yaml
                      helm repo add stable https://kubernetes-charts.storage.googleapis.com
		      helm repo update
		      helm install prometheus-operator stable/prometheus-operator --namespace monitor --set grafana.service.type=NodePort -f values.yaml
                  """
              }
          }
      }
  }
}
