pipeline{

    agent any

    environment{

        DOCKER_TAG = getDockerTag()

        registry = "artifactory/docker-1-local"

    }

    stages{

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "artifactory-server",
                    url: "//sunayana.jfrog.io/",
                    credentialsId: "jfrog_credentials"
                )
            }
        }

        stage ('Build docker image') {
            steps {
                script {
                    docker.build(registry + '/helloimage:latest')
                }
            }
        }

        stage ('Push image to Artifactory') {
            steps {
                rtDockerPush(
                    serverId: "artifactory-server",
                    image: registry + '/helloimage:latest',
                    // Host:
                    // On OSX: "tcp://127.0.0.1:1234"
                    // On Linux can be omitted or null
                    //host: HOST_NAME,
                    targetRepo: 'docker-1-local',
                    // Attach custom properties to the published artifacts:
                    properties: 'project-name=docker-1;status=stable'
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "artifactory-server"
                )
            }
        }
        
    }

}

def getDockerTag(){

    def tag = sh script:'git rev-parse HEAD', returnStdout: true

    return tag
}