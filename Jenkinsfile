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
  - name: pawst
    image: bryandollery/terraform-packer-aws-alpine
    tag: 2
    tty: true
    cmd: bash
"""
    }
  }
  environment {
    CREDS = credentials('bryan_aws_creds')
    AWS_ACCESS_KEY_ID = "${CREDS_USR}"
    AWS_SECRET_ACCESS_KEY = "${CREDS_PSW}"
    OWNER = 'bryan'
    PROJECT_NAME = 'web-server'
  }
  stages {
    stage("build") {
      steps {
        sh 'packer build packer.json'
      }
    }
  }
  post {
    success {
        build quietPeriod: 0, wait: false, job: 'bryan-jenkins-lab-2-tf'  
    }
  }
}
