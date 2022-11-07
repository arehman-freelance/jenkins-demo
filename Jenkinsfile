pipeline {
    environment {
        imagename = "mavenarh/mynginx"
        registryCredential = 'mavenarh-dockerhub'
        dockerImage = ''
      }    
    agent any

    stages {
        // stage('Code Pull') {
        //     steps {
        //         echo 'Pulling the code'
        //         git credentialsId: 'gitea-smart', url: 'https://git.smartparadigm.net/arehman/jenkins-demo.git'
        //     }
        // }
        stage('Docker Build') {
            steps {
                echo 'Build and tag image'
                // sh 'docker build -t mavenarh/mynginx:latest .'
                script {
                    dockerImage = docker.build imagename
                }
            }
        }    
        stage('Push Image') {
            steps {
                echo 'Build and tag image'
                // sh 'docker push mavenarh/mynginx:latest'
                script {
                        docker.withRegistry( '', registryCredential ) {
                        // dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                        }
                }                
            }
        }   
        stage('Deploy') {
            steps {
                echo 'Deploying to microk8s'
                //sh 'kubectl apply --kubeconfig /home/confs/jenkins-demo/kube_config -f deployment.yaml'
                sh 'kubectl --kubeconfig /home/confs/jenkins-demo/kube_config delete pods -l app=nginx'
            }
        }            
    }
}
