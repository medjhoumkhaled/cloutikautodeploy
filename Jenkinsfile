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
                withCredentials([sshUserPrivateKey(credentialsId: 'eu-server', keyFileVariable: 'EU_prevatefile')]) {
                    sh 'scp -oStrictHostKeyChecking=no -i ${EU_prevatefile} jktest.html root@login.cloutik.eu:/var/www/html/cloutik/'
                }
            }
        }

        stage('Deploy in remote com webserver'){
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'com-server', keyFileVariable: 'COM_prevatefile')]) {
                    sh 'scp -oStrictHostKeyChecking=no -i ${COM_prevatefile} jktest.html root@login.cloutik.com:/var/www/html/cloutik/'
                }        
            }
        }
        
        stage('Deploy in remote us webserver'){
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'us-server', keyFileVariable: 'US_prevatefile')]) {
                    sh 'scp -oStrictHostKeyChecking=no -i ${US_prevatefile} jktest.html root@login.cloutik.us:/var/www/html/cloutik/'
                }
            }
        }
        
        stage('Deploy in remote asia webserver'){
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'asia-server', keyFileVariable: 'ASIA_prevatefile')]) {
                    sh 'scp -oStrictHostKeyChecking=no -i ${ASIA_prevatefile} jktest.html root@login.cloutik.asia:/var/www/html/cloutik/'
                }
            }
        }
    }
}

def getVersion() {
     def v= sh returnStdout: true, script: 'git rev-parse --short HEAD'
     return v
}
