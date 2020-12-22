import groovy.transform.Field
podTemplate(name: 'node-test', label: 'test', cloud: 'local-kubernetes', yaml: """
kind: Pod
metadata:
  name: newman
spec:
  containers:
  - name: newman
    image: postman/newman
    imagePullPolicy: Always
    workingDir: /opt
    command:
    - cat
    tty: true
""") {
    node('test') {
        stage('clone') {
            timestamps {
                sshagent(['jenkins_key']) {
                    git(
                        url: 'ssh://git@bitbucket.org/wyzestudio/ftx-api-gateway-deployment.git',
                        branch: GIT_BRANCH,
                        credentialsId: 'jenkins_key')
                }
            }
        }

        stage ('running newman') {
            container('newman') {
                sh 'newman .json'
            }
        }
    }
}