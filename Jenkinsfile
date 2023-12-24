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
 
        stage('Start Prometheus and Grafana') {
            steps {
                script {
                    // Start Prometheus container
                    sh "docker run -d --name $prometheusContainerName -p 9090:9090 -v $prometheusConfigPath:/etc/prometheus/prometheus.yml prom/prometheus"

                    // Start Grafana container
                    sh "docker run -d --name $grafanaContainerName -p 3000:3000 grafana/grafana"
                }
            }
        }
 stage('Ansible Deployment') {
      ansiblePlaybook credentialsId: 'ansibleid1', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
        }
 stage('Configure Monitoring') {
    steps {
        script {
            // Assuming your Ansible deployment sets up some monitoring-related configurations
            // Update Prometheus configuration to scrape metrics from your deployed services
           
            // Wait for Prometheus to reload the configuration
           

            // Wait for some time to allow Prometheus to discover and scrape targets

            // Update Grafana dashboards to visualize the metrics
            // You may use Grafana API or CLI to import dashboards
            // Example using Grafana API (replace the API key and dashboard JSON with your own)
            
        }
    }
}

    }


