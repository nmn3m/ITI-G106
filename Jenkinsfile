pipeline { 
    environment { 
        registry = "357812/jenkins" 
        registryCredential = 'dockerhub_id' 
        dockerImage = '' 
    }
    agent{label "mn3m"}  
	tools {
		maven 'maven-3.8.6' 
	}
    stages { 
        stage('checkout') { 
            steps { 
                git url: 'https://github.com/nmn3m/ITI-G106.git', branch: 'master'
            }
        } 
		stage('building'){
			steps {
				sh 'mvn clean package'
			}
		}
		stage('Testing'){
			steps {
				sh 'mvn test'
			}
			post {
				always {
					 junit '**/target/surefire-reports/TEST-*.xml'
				}

			}
		}
        stage('Building Image && Deploying') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 

                    docker.withRegistry( '', registryCredential ){
                        dockerImage.push() 
					}
				} 
			}
		}
    }
}
