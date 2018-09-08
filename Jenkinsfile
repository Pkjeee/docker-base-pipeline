node{
timestamps {
        properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '2', numToKeepStr: '2'))])
        stage('\u2780 Git Checkout'){
        git 'https://github.com/Pkjeee/docker-base-pipeline.git'
        }
        stage('\u2781 Maven Package'){
            def mvnHome = tool name: 'MAVEN', type: 'maven'
            sh "${mvnHome}/bin/mvn package"
        }
        stage('\u2782 Build Docker Images'){
            sh 'docker build -t pramodvishwakarma/my-app:1.0.0 .'
        }
        stage('\u2783 Push Docker Image'){
            withCredentials([string(credentialsId: 'Docker-Pass', variable: 'DockerHubPass')]) {
            sh "docker login -u pramodvishwakarma -p ${DockerHubPass}" 
            }
            sh 'docker push pramodvishwakarma/my-app:1.0.0'
        }
        stage('\u2784 Remove Containers'){
            def DockerRm = 'docker rm -f My-Tomcat-App'
            sshagent(['SSH-KEY-102']) {
            sh "ssh -o StrictHostKeyChecking=no root@192.168.56.102 ${DockerRm}"            
        }
        stage('\u2785 Deploy New Container'){
            def DockerRun = 'docker run -p 8080:8080 -d --name My-Tomcat-App pramodvishwakarma/my-app:1.0.0'
            sshagent(['SSH-KEY-102']) {
            sh "ssh -o StrictHostKeyChecking=no root@192.168.56.102 ${DockerRun}"
            }
        }
    }
  }
}
