podTemplate(cloud:'openshift',
yaml: """
kind: Pod
spec:
  containers:
  - name: maven
    image: docker.io/maven:3-jdk-8
    command: ['cat']
    tty: true
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command: ['cat']
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /kaniko/.docker
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: regcred
          items:
            - key: .dockerconfigjson
              path: config.json
"""
){
    node(POD_LABEL) {
        stage('Checkout'){
            git 'https://github.com/LancelotTian/ping_pong'
        }

        container('maven'){
            stage('Compile & Unit Test & Package') {
                sh 'mvn -B clean install'
            }
        }

        container('kaniko'){
            stage('Docker build & Push to registry'){
                sh 'cat /kanik/.docker/config.json'
                sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=quay-default.apps.cluster-beijing-1fdf.beijing-1fdf.example.opentlc.com/quay/ping-pong:$BUILD_ID'
            }
        }

    }
}
