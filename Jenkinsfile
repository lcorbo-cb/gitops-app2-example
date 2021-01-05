pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('Run maven') {
      steps {
        container('maven') {
          sh 'mvn -version'
          sh 'echo "I am running on an agent!"'
        }
        container('busybox') {
          sh '/bin/busybox'
        }
      }
    }
  }
  post {
      always {
        container('busybox') {
          sh 'echo "I will always say Hello again!"'
        }
      }
      failure {
        container('busybox') {
          sh 'echo "I failed!"'
        }
      }
      success {
        container('busybox') {
          sh 'echo "I completed!"'
        }
      }
      cleanup {
        container('busybox') {
          sh 'echo "I am going to clean up these files."'
        }
      }
  }
}
