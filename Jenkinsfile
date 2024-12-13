node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALS', url:  'https://github.com/Danle360/spring-boot-mongo-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t danle360/spring-boot-mongo .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker_pwd', variable: 'docker_pwd')]) {
          sh "docker login -u danle360 -p ${docker_pwd}"
        }
        sh 'docker push danle360/spring-boot-mongo'
     }
     
     stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'KUBERNATES_CONFIG',
         enableConfigSubstitution: true
        )
     }
	 
	  /**
      stage("Deploy To Kuberates Cluster"){
        sh 'kubectl apply -f springBootMongo.yml'
      } **/
     
}


