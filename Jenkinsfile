pipeline{

    agent any

    environment{

        DOCKER_TAG = getDockerTag()

    }

    stages{

        

        // stage ('Artifactory configuration') {

        //     agent {label 'jf-scan-docker-images'}

        //     steps {
        //         rtServer (
        //             id: "artifactory-server",
        //             url: "//devika.jfrog.io/artifactory",
        //             credentialsId: "devikajfrog"
        //         )
        //     }
        // }

        stage ('Artifactory configuration installation') {

            agent {label 'jf-scan-docker-images'}

        steps{
        sh '''

            curl -fL https://getcli.jfrog.io | bash -s v2

            ./jfrog c add --url=https://devika.jfrog.io --user=sunayanareddy1106@gmail.com --password=cmVmdGtuOjAxOjE2ODk3NzA4MjU6bHQ4aHpUY3FsaVNLN3dSZDBTYzJQMnZ0bmp4 --interactive=false --overwrite

            ./jfrog pipc --server-id-resolve=repo-dev --repo-resolve=pypi

        '''
        }

        }
        

        stage('Build Docker Image'){

            //agent {label 'jf-scan-docker-images'}

            steps{

                sh "sudo docker build . -t devika.jfrog.io/docker-local/helloimage:${DOCKER_TAG}"
            }
        }

        stage ('DockerImage Push to Jfrog'){

            //agent {label 'jf-scan-docker-images'}

            steps{

                withCredentials([string(credentialsId: 'devikafrog', variable: 'jfrogPwd')]) {

                    sh "docker login devika.jfrog.io -u sunayanareddy1116@gmail.com -p ${jfrogPwd}"

                    sh "docker push devika.jfrog.io/docker-local/helloimage:${DOCKER_TAG}"
                }
            }

        }

        stage ('Jfrog DockerImage Pull'){

            agent {label 'jf-scan-docker-images'}

            steps{

                withCredentials([string(credentialsId: 'devikajfrog', variable: 'jfrogPwd')]) {

                    sh "docker login devika.jfrog.io -u sunayanareddy1116@gmail.com -p ${jfrogPwd}"

                    sh "docker pull devika.jfrog.io/docker-local/helloimage:${DOCKER_TAG}"
                }
            }

        }

        stage ('Xray scan') {

            agent {label 'jf-scan-docker-images'}

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