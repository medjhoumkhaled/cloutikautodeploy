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
                sshagent(['eu-server']) {
                    sh 'scp -oStrictHostKeyChecking=no jktest.html root@login.cloutik.eu:/var/www/html/cloutik/'
                }
            }
        }
        
        stage('Deploy in remote us webserver'){
            steps {
                sshagent(['us-server']) {
                    sh 'scp -oStrictHostKeyChecking=no jktest.html root@login.cloutik.us:/var/www/html/cloutik/'
                }
            }
        }
        
        stage('Deploy in remote asia webserver'){
            steps {
                sshagent(['asia-server']) {
                    sh 'scp -oStrictHostKeyChecking=no jktest.html root@login.cloutik.asia:/var/www/html/cloutik/'
                }
            }
        }
    
        stage('Deploy in remote com webserver'){
            steps {
                sshagent(['com-server']) {
                    sh 'scp -oStrictHostKeyChecking=no jktest.html root@login.cloutik.com:/var/www/html/cloutik/'
                }        
            }
        }
    }
}

def getVersion() {
     def v= sh returnStdout: true, script: 'git rev-parse --short HEAD'
     return v
}
