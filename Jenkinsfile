pipeline{

    agent {label 'jf-scan-docker-images'}

    environment{

        DOCKER_TAG = getDockerTag()

    }

    stages{

        

        stage ('Artifactory configuration') {

            steps {
                rtServer (
                    id: "artifactory-server",
                    url: "//devika.jfrog.io/artifactory",
                    credentialsId: "devikajfrog"
                )
            }
        }

        stage ('Artifactory configuration installation') {

        steps{
        sh '''

            curl -fL https://getcli.jfrog.io | bash -s v2

            ./jfrog c add --url=https://devika.jfrog.io --user=sunayanareddy1106@gmail.com --password=AKCp8mZ8Sd4zdC4SwJzvNSXmsqYKmwbodx5YLygkH9U4NndtAMKrtz4Y3D9bE9aySm34SQYWa --interactive=false --overwrite

            ./jfrog pipc --server-id-resolve=repo-dev --repo-resolve=pypi

        '''
        }

        }
        

        stage('Build Docker Image'){

            steps{

                sh "docker build . -t devika.jfrog.io/docker-local/helloimage:${DOCKER_TAG}"
            }
        }

        stage ('DockerImage Push to Jfrog'){

            steps{

                withCredentials([string(credentialsId: 'devikafrog', variable: 'jfrogPwd')]) {

                    sh "docker login devika.jfrog.io -u sunayanareddy1116@gmail.com -p ${jfrogPwd}"

                    sh "docker push devika.jfrog.io/docker-local/helloimage:${DOCKER_TAG}"
                }
            }

        }

        stage ('Jfrog DockerImage Pull'){

            steps{

                withCredentials([string(credentialsId: 'devikajfrog', variable: 'jfrogPwd')]) {

                    sh "docker login devika.jfrog.io -u sunayanareddy1116@gmail.com -p ${jfrogPwd}"

                    sh "docker pull devika.jfrog.io/docker-local/helloimage:${DOCKER_TAG}"
                }
            }

        }

        stage ('Xray scan') {

            steps {
                
                sh "jf docker scan devika.jfrog.io/docker-local/helloimage:${DOCKER_TAG} --format=json" 
            }
        }
        
    }

}

def getDockerTag(){

    def tag = sh script:'git rev-parse HEAD', returnStdout: true

    return tag
}