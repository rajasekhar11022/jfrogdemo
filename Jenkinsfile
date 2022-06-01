pipeline{

    agent any

    environment{

        DOCKER_TAG = getDockerTag()

    }

    stages{

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "artifactory-server",
                    url: "//sunayana.jfrog.io/artifactory",
                    credentialsId: "sunayanajfrog"
                )
            }
        }

        stage('Build Docker Image'){

            steps{

                sh "docker build . -t docker-local/helloimage:${DOCKER_TAG}"
            }
        }

        stage ('DockerImage Push to Jfrog'){

            steps{

                withCredentials([string(credentialsId: 'sunayanajfrog', variable: 'jfrogPwd')]) {

                    sh "docker login sunayana.jfrog.io -u sunayanareddy1116@gmail.com -p ${jfrogPwd}"

                    sh "docker push docker-local/helloimage:${DOCKER_TAG}"
                }
            }

        }

        stage ('Jfrog DockerImage Pull'){

            steps{

                withCredentials([string(credentialsId: 'sunayanajfrog', variable: 'jfrogPwd')]) {

                    sh "docker login sunayana.jfrog.io -u sunayanareddy1116@gmail.com -p ${jfrogPwd}"

                    sh "docker pull sunayana.jfrog.io/docker-local/hello-world"
                }
            }

        }

        //stage ('Build docker image') {
          //  steps {
            //    script {
              //      docker.build(registry + '/helloimage:latest')
                //}
            //}
        //}
        
    }

}

def getDockerTag(){

    def tag = sh script:'git rev-parse HEAD', returnStdout: true

    return tag
}