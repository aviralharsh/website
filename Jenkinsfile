podTemplate(label: 'mypod', containers: [
     containerTemplate(name: 'git', image: 'alpine/git', ttyEnabled: true, command: 'cat'),
     containerTemplate(name: 'maven', image: 'maven:3.3.9-jdk-8-alpine', command: 'cat', ttyEnabled: true),
     containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true)
    ],
    volumes: [
        hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
    ]
    ) 
    properties([pipelineTriggers([githubPush()])])
      {
      node('mypod') {
            stage('Check running containers') {
            container('docker') {
                sh 'hostname'
                sh 'docker images'
                git 'https://github.com/aviralharsh/website.git'
            }
        }

        stage('Clone repository') {
            container('git') {
                sh 'git clone -b master https://github.com/aviralharsh/website.git'
            }
        }

        //stage('Maven Build') {
        //    container('maven') {
        //        dir('website/') {
        //            sh 'hostname'
        //            sh 'mvn clean install'
        //        }
        //    }
        //}
        
        stage("Build image") {
            container('docker'){
                dir('website/'){
                    sh 'pwd'
                    sh 'ls'
                    sh 'docker build -t aviralharsh05/website .'
                }
            }
        }
        
        stage("Push image") {
            container('docker'){
                dir('website/'){
                    
                    sh 'docker login -u aviralharsh05 -p br01bd6195'
                    sh 'docker push aviralharsh05/website'
                    sh 'pwd'
                    
                }
            }
        }
    }  
}
