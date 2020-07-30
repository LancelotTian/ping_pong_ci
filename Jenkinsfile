podTemplate(
    cloud: 'openshift',
    inheritFrom: 'maven'
) {
  node(POD_LABEL) {
    stage('Get a Maven project') {
      git 'https://github.com/LancelotTian/ping_pong'
      sh 'mvn -B clean install'
    }
 }
}
