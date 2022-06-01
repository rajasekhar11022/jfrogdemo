pipeline{

    agent any

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

        stage ('Jfrog DockerImage Pull'){

            steps{

                withCredentials([string(credentialsId: 'sunayanajfrog', variable: 'jfrogPwd')]) {

                    sh "docker login sunayana.jfrog.io -u sunayanareddy1116@gmail.com -p ${jfrogPwd}"
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

        //stage ('Pull an image from Artifactory') {
            //steps {
               // rtDockerPull(
                  //  serverId: "artifactory-server",
                //    image:  registry + '/hello-world:latest',
                    // Host:
                    // On OSX: "tcp://127.0.0.1:1234"
                    // On Linux can be omitted or null
                    //host: HOST_NAME,
              //      sourceRepo: 'docker-local'
            //    )
          //  }
        //}

        //stage ('Publish build info') {
          //  steps {
            //    rtPublishBuildInfo (
              //      serverId: "artifactory-server"
                //)
            //}
        //}
        
    }

}