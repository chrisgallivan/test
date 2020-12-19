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
        imageName = loadValuesYaml('imageName')
	slackChannel = loadValuesYaml('slackChannel')
	registryCredential = loadValuesYaml('registryCredential')
        dockerImage = loadValuesYaml('dockerImage')
        backendFile = loadValuesYaml('backendFile')
        backendPath = loadValuesYaml('backendPath')
	successAction = loadValuesYaml('successAction')
	failureAction = loadValuesYaml('failureAction')    
	   
    }
    agent any
    stages {
        stage('Slack Notification'){
            steps {
		    slackSend channel: "${slackChannel}",
                    color: COLOR_MAP[currentBuild.currentResult],
			    message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}\n APP_URL:http://a0a87a88d82c2429ca00693710427340-1289019772.us-east-2.elb.amazonaws.com/url"
            }
        }    
    }
    post {
    
        success {
            sh "${successAction}"
        
        }
    
        failure {
		sh "${failureAction}"
        
        }
    }
}
