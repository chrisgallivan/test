def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]

def loadValuesYaml(x){
  def valuesYaml = readYaml (file: './pipeline.yml')
  return valuesYaml[x];
}
pipeline {
    environment {
        imageName = "chrisgallivan/automate-all-the-things-docker"
        registryCredential = 'docker_hub'
        dockerImage = ''
        BACKEND_FILE = "terraformConfig.tf"
        BACKEND_PATH = "global/s3/terraform.tfstate"
	    BUILD_USER = ''    
    }
    agent any
    stages {
        stage('Slack Notification'){
            steps {
		        slackSend channel: '#general-old',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}\n APP_URL:http://a0a87a88d82c2429ca00693710427340-1289019772.us-east-2.elb.amazonaws.com/url "
            }
        }    
    }
    post {
    
        success {
            sh "curl -X POST https://maker.ifttt.com/trigger/jenkins_success/with/key/geqAu4WK7xgLcr4md038_CrKCsXrRjZbJ9MsQBwPLg9"
        
        }
    
        failure {
            sh "curl -X POST https://maker.ifttt.com/trigger/jenkins_failure/with/key/geqAu4WK7xgLcr4md038_CrKCsXrRjZbJ9MsQBwPLg9"
        
        }
    }
}
