currentBuild.displayName = "Cloutik-git-pipline#" + currentBuild.number 
pipeline {
    agent any
    environment {
         DOCKER_TAG = getVersion()
    }

    stages {
        stage('Data settings'){
            steps {
                sh 'chmod +x updateIndexVersion.sh'
                sh './updateIndexVersion.sh ${DOCKER_TAG} jktest.html'

            }
        }
        
        stage('Deploy in remote eu webserver'){
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'com-server', keyFileVariable: 'EU_prevatefile')]) {
                    sh 'scp -oStrictHostKeyChecking=no -i ${EU_prevatefile} jktest.html root@login.cloutik.eu:/root/'
                }
                
                
                
            }
        }

    }
}

def getVersion() {
     def v= sh returnStdout: true, script: 'git rev-parse --short HEAD'
     return v
}
