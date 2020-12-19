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
	slackMessage = loadValuesYaml('slackMessage')
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
			    message: ${slackMessage}
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
