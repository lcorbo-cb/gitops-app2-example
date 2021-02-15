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
  triggers {
    eventTrigger jmespathQuery("eventName=='newbuild'" )
  }
  stages {
    stage('Run maven') {
      steps {
        echo "buildtag:" + currentBuild.getBuildCauses()[0].event.buildTag.toString()
        echo "source:" + currentBuild.getBuildCauses()[0].event.buildTag.source.toString()
      //  buildtag:{"eventName":"newbuild","buildTag":"corbolj2","source":{"type":"JenkinsBuild","buildInfo":{"build":12,"job":"sailpoint/master","jenkinsUrl":"https://cbci.cloudbees.demo/master01/","instanceId":"53edc47ec94fb8cb4a456a2a3802f5bb"}}}

        // echo currentBuild.getBuildCauses()[0].event.toString()
        container('maven') {
          sh 'mvn -version'
          sh 'echo "I am running on an agent!"'
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
