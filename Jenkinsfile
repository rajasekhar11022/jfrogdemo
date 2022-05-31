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

        stage('Build Docker Image'){

            steps{

                sh "docker build . -t rajasekhar11022/helloimage:${DOCKER_TAG}"
            }
        }

        stage ('Push image to Artifactory') {
            steps {
                rtDockerPush(
                    serverId: "artifactory-server",
                    image: rajasekhar11022 + '/hello-world:${DOCKER_TAG}',
                    // Host:
                    // On OSX: "tcp://127.0.0.1:1234"
                    // On Linux can be omitted or null
                    //host: HOST_NAME,
                    targetRepo: 'docker-local',
                    // Attach custom properties to the published artifacts:
                    //properties: 'project-name=docker1;status=stable'
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }

    }

}

def getDockerTag(){

    def tag = sh script:'git rev-parse HEAD', returnStdout: true

    return tag
}