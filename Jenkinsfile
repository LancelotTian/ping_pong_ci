podTemplate(cloud:'openshift',
            containers: [
        containerTemplate(name: 'maven', image: 'docker.io/maven:3-jdk-8', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'sonar', image: 'docker.io/sonarsource/sonar-scanner-cli', ttyEnabled: true, command: 'cat', envVars: [
            envVar(key: 'SONAR_HOST_URL', value: 'http://sonarqube-default.apps.cluster-beijing-959a.beijing-959a.example.opentlc.com')]),
        containerTemplate(name: 'docker', image: 'docker.io/docker:19', command: 'sleep',args: '99d',envVars: [
            envVar(key: 'DOCKER_HOST', value: 'tcp://localhost:2375')]),
        containerTemplate(name: 'docker-daemon', image: 'docker.io/docker:19-dind', privileged: true,envVars: [
            envVar(key: 'DOCKER_TLS_CERTDIR', value: '')])
    ]
){
    node(POD_LABEL) {
        stage('Checkout'){
            git 'https://github.com/LancelotTian/ping_pong'
        }

        container('maven') {
            stage('Compile & Unit Test & Package') {
                sh 'mvn -B clean install'
            }
        }

        container('sonar') {
            stage('Code analysis') {
                sh 'sonar-scanner'
            }
        }

        container('docker'){
            stage('Docker build'){
                docker.build("ping-pong:${env.BUILD_ID}")
            }
        }
    }
}
