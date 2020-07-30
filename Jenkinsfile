podTemplate(
    cloud: 'openshift',
    yaml: """
apiVersion: v1
kind: Pod
metadata:
  labels:
    jenkins-node: jenkins-node
spec:
  containers:
  - name: maven
    image: image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-maven:latest
    imagePullPolicy: Always
    tty: true
"""
) {
  node(POD_LABEL) {
    stage('Get a Maven project') {
      git 'https://github.com/LancelotTian/ping_pong'
      container('maven') {
        stage('Build a Maven project') {
          sh 'mvn -B clean install'
        }
      }
    }
 }
}
