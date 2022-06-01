pipeline{

    agent any

    environment{

        DOCKER_TAG = getDockerTag()

        registry = "sunayana.jfrog.io/artifactory/docker-local"

    }

    stages{

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "artifactory-server",
                    url: "//sunayana.jfrog.io/artifactory",
                    credentialsId: "jfrog_credentials"
                )
            }
        }

        //stage ('Build docker image') {
          //  steps {
            //    script {
              //      docker.build(registry + '/helloimage:latest')
                //}
            //}
        //}

        stage ('Pull an image from Artifactory') {
            steps {
                rtDockerPull(
                    serverId: "artifactory-server",
                    image:  registry + '/hello-world:latest',
                    // Host:
                    // On OSX: "tcp://127.0.0.1:1234"
                    // On Linux can be omitted or null
                    //host: HOST_NAME,
                    sourceRepo: 'docker-local'
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