podTemplate(cloud:'openshift',
            containers: [
        containerTemplate(name: 'maven', image: 'docker.io/maven:3-jdk-8', ttyEnabled: true, command: 'cat'),
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

        container('docker'){
            stage('Docker build'){
                docker.build("ping-pong:${env.BUILD_ID}")
            }
        }
    }
}
