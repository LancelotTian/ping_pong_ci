podTemplate(
    cloud: 'openshift',
    inheritFrom: 'maven'
) {
  node('maven') {
    stage('Get a Maven project') {
      git 'https://github.com/LancelotTian/ping_pong'
      sh 'mvn -B clean install'
    }
 }
}
