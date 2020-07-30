pipeline {
    agent {
    kubernetes {
        label 'podlabel'
        yaml """
apiVersion: v1
kind: Pod
metadata:
  name: jenkins-agent
spec:
  containers:
    - name: jenkins-agent
    image: registry.redhat.io/openshift4/ose-jenkins-agent-maven

  imagePullSecrets:
    - name: 12858982-leon--pull-secret
"""
   }
        }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}