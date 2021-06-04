node{
    

    def mvnHome =  tool name: 'maven3', type: 'maven' 
    environment(branch='qa')
    stage('git chkout'){
        
    git branch: "${params.br}", url: 'https://github.com/KSDJ10/my-app.git'
    echo "${params.br}"
    echo "${env.WORKSPACE}"
    }
    stage('build'){
         
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/*.war target/newapp.war'
    }
    stage('build docker image'){
        sh 'docker build -t ksdj10/mywebapp:0.0.1 .'
    }
    stage('push docker image'){
        withCredentials([string(credentialsId: 'dockerpass', variable: 'dockerPassword')]) {
            sh "docker login -u ksdj10 -p ${dockerPassword}"
            
    sh 'docker push ksdj10/mywebapp:0.0.1'
}
    }
    stage('Docker deployment')
    {
   sh 'docker run -d -p 8090:8080 --name tomcattest ksdj10/mywebapp:0.0.1' 
   }
}
