pipeline {
  agent any
  
   tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_creds', url: 'https://github.com/karthibitcot11/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline/']])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t karthi2611/devopshint .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'devopshintdocker', variable: 'devopshintdocker')]) {
                    sh 'docker login -u karthi2611 -p ${devopshintdocker}'
            }
            sh 'docker push karthi2611/devopshint'
        }
            }   
        }
         
     stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name demo-ekscluster --region us-east-1')
          sh "kubectl get ns"
          sh "kubectl apply -f nodejsapp.yaml"
        }
      }
    }

  }
}
