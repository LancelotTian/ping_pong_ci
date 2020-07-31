podTemplate(cloud:'openshift',
            containers: [
    containerTemplate(name: 'maven', image: 'docker.io/maven:3-jdk-8', ttyEnabled: true, command: 'cat')
    ])
{
    node(POD_LABEL) {
        git 'https://github.com/LancelotTian/ping_pong'
        container('maven') {
            stage('Build a Maven project') {
                sh 'mvn -B clean install'
            }
        }
    }
}
