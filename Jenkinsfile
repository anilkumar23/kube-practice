pipeline {
  agent any
  stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace... */
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        sh 'echo $USER'
        sh 'echo whoami'
      }
    }
    stage('Push image to aws ecr'){
      steps {
       withDockerRegistry(credentialsId: 'ecr:us-east-1:aws-credentials', url: 'http://941609391146.dkr.ecr.us-east-1.amazonaws.com/example') {
       sh 'docker tag anil9848/account-service:latest 941609391146.dkr.ecr.us-east-1.amazonaws.com/example'
       sh 'docker tag anil9848/account-service:latest 941609391146.dkr.ecr.us-east-1.amazonaws.com/example'
         sh 'docker push 941609391146.dkr.ecr.us-east-1.amazonaws.com/example'
}
      }
    }
    stage('Run docker image on kubernetes cluster') {
      steps {
        node('EKS-master'){
          checkout scm
         sh 'aws eks --region us-east-1 update-kubeconfig --name eks-master'
         sh 'kubectl apply -f deployment.yaml'
         sh 'kubectl apply -f service.yaml'
        }
      }
    }
  }
}
