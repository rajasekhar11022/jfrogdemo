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

                sh "docker build . -t docker-local/helloimage:${DOCKER_TAG}"
            }
        }

        stage('JfrogRepo Push'){

            steps{

               withCredentials([usernameColonPassword(credentialsId: 'jfrog_credentials', variable: 'jfrog-repo')]) {

                    sh "docker login -u sunayana.jfrog.io -p ${jfrog-repo}"

                    //sh "docker push rajasekhar11022/helloimage:${DOCKER_TAG}"
                }
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