node(){
 
    

    
     
    stage('code checkout'){
     checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'githubcred', url: 'https://github.com/SiyaaJhawar/star-agile-health-care']])
    }
    stage('Maven Build'){
        sh """  
           mvn clean install
           """
          
    }
}
