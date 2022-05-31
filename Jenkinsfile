pipeline{

    agent any

    environment{

        DOCKER_TAG = getDockerTag()

        registry = artifactory/docker-1-local

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

        stage ('Build docker image') {
            steps {
                script {
                    docker.build(registry + '/helloimage:latest')
                }
            }
        }
        
    }

}

def getDockerTag(){

    def tag = sh script:'git rev-parse HEAD', returnStdout: true

    return tag
}