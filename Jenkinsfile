template = '''
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: docker
  name: docker
spec:
  containers:
  - command:
    - sleep
    - "3600"
    image: docker
    name: docker
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker
  volumes:
  - name: docker
    hostPath:
      path: /var/run/docker.sock
    '''

podTemplate(cloud: 'kubernetes', label: 'docker', yaml: template) {
node("docker") {
    container("docker") {
    stage("Checkout SCM") { 
        git branch: 'main', url: 'https://github.com/kaizenacademy/flask-app-build.git'
    } 

    withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {

    stage("Docker login") {
        sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
    }

    stage("Docker build") {
        sh "docker build -t ${DOCKER_USER}/jenkins-flask-app:1.0.0 ."
    }

    stage("Docker push") {
        sh "docker push ${DOCKER_USER}/jenkins-flask-app:1.0.0"
    }
    }
    }
}
}