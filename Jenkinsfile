podTemplate(
    cloud: 'openshift'
    name: 'maven'
) {
  node(POD_LABEL) {
    stage('Get a Maven project') {
      git 'https://github.com/LancelotTian/ping_pong'
      container('jnlp') {
        stage('Build a Maven project') {
          sh 'mvn -B clean install'
        }
      }
    }
 }
}
