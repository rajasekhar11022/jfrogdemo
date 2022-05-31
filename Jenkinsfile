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
                    url: //sunayana.jfrog.io/
                    credentialsId = "jfrog_credentials"
                )
            }
        }

        stage('DockerHub Push'){

            steps{

                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {

                    sh "docker login -u rajasekhar11022 -p ${dockerHubPwd}"

                    sh "docker push rajasekhar11022/helloimage:${DOCKER_TAG}"
                }
            }
        }

    }

}

def getDockerTag(){

    def tag = sh script:'git rev-parse HEAD', returnStdout: true

    return tag
}