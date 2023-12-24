node(){
 
    
def containerName="healthins"
def tag="latest"
def dockerHubUser="swatig139627"
    
     
    stage('code checkout'){
     checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'githubcred', url: 'https://github.com/SiyaaJhawar/star-agile-health-care']])
    }
    stage('Maven Build'){
        sh """  
           mvn clean install
           """
          
    }
  stage("Image Prune"){
         sh "docker image prune -f"
    }

    stage('Image Build'){
        sh "docker build -t $containerName:$tag --pull --no-cache ."
        echo "Image build complete"
    }

    stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
            sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
        }
}
 
 stage('Ansible Deployment') {
      ansiblePlaybook credentialsId: 'ansibleid1', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
        }
 stage('Configure Monitoring') {
    steps {
        script {
            sh "echo '  - job_name: Ansible slave' >> $prometheusConfigPath"
            sh "echo '    static_configs:' >> $prometheusConfigPath"
            sh "echo '      - targets: [\"172.31.3.191:9100\"]' >> $prometheusConfigPath"

            // Wait for some time to allow Prometheus to discover and scrape targets

            // Update Grafana dashboards to visualize the metrics
            // You may use Grafana API or CLI to import dashboards
            // Example using Grafana API (replace the API key and dashboard JSON with your own)
            
        }
    }
}

    }


