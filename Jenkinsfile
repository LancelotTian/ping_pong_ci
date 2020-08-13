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
          name: quayio-regcred
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
                sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=$REGISTRY_PATH/ping-pong:$BUILD_ID'
            }
        }

        stage('Update deployment'){
            git credentialsId: 'git-ssh-key', url: 'git@github.com:LancelotTian/ping_pong_gitops.git'
            sh 'sed -i "s/ping-pong:.*$/ping-pong:$BUILD_ID/g" deployment.yaml'
            sh 'git config --global user.email "tianliang1980@gmail.com"'
            sh 'git config --global user.name "lancelottian"'
            sh 'git commit deployment.yaml -m "Update by Jenkins pipeline"'
            sshagent(['git-ssh-key']) {
                sh 'git push --set-upstream origin master'
            }
        }
    }
}
