pipeline{

agent any

tools{
maven 'Maven3..9.5'

}

  stages{

  stage('CheckOutCode'){
    steps{
    git credentialsId: 'github_credential', url: 'https://github.com/Amargunge/spring-petclinic.git'
	
	}
  }
  stage('Build'){
  steps{
  sh  "mvn clean package"
  }
  }

  stage('Build Docker Image'){
    steps{
       sh 'docker build -t Amargunge/spring-petclinic .'
    }
  }

  stage('Push Docker Image'){
    withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          sh "docker login -u Amargunge -p ${DOKCER_HUB_PASSWORD}"
        }
        sh 'docker push Amargunge/spring-petclinic'
     }

      stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'Kubernete Manifast.yml', 
         kubeconfigId: 'KUBERNATES_CONFIG',
         enableConfigSubstitution: true
        )
     }
