pipeline {
	agent any
		
		environment {
			DockerUser = credentials('DockerUser')
			DockerPass = credentials('DockerPass')
		}
		stages {                 
			stage('Prepare') {                         
				steps {                                 
					echo 'Preparing..'
				}                 
			}                 
			stage('Build') {                         
				steps {                                 
					echo 'Building..'
					//sh 'if [ "$(docker images ${reg} -q)" != "" ]; then docker rmi -f $(docker images ${reg} -q --no-trunc); fi'
					sh 'docker build --rm . -t cristiancristancho/rampup_back'                         
				}                 
			} 
			/*               
			stage('Test') {                         
				steps {                                 
					echo 'Testing...'  
					sh 'docker run --rm --name movieUIjenkinsTest -d challengejenkins:${BUILD_NUMBER} npm test'                        
				}                 
			}*/
			
			stage('push') {
				steps {
					echo 'pushing'
					sh 'docker login -u $DockerUser -p $DockerPass'
					sh 'docker tag cristiancristancho/rampup_back cristiancristancho/rampup_back:${BUILD_NUMBER}'
					sh 'docker push cristiancristancho/rampup_back:${BUILD_NUMBER}'

				}
			}
			/* stage('Check') {                         
				steps {                                 
					echo 'Look the new version on port 4000....'  
					input 'check new version?'
					//sh 'docker stop $(docker ps -aq)'
					//sh 'docker rm $(docker ps -aq)'
					//sh 'docker rmi $( docker images | grep "^<none>" | awk "{print $3}" )'
					//sh 'docker stop challjenkNew'
					//sh 'docker rm challjenkNew'
					sh 'docker run --rm --name movieApiNew -d -p 4000:3000 cristiancristancho/rampup_back:${BUILD_NUMBER}'  
                                 					
				}                 
			} */

			/* stage('Deploy') {                         
				steps {                                 
					echo 'Deploying....'  
					input 'Accept deployment?'
					//sh 'docker stop $(docker ps -aq)'
					sh 'docker stop movieApiNew'
					sh 'if [ "$(docker ps -f name=movieApi" != " " ]; then docker stop movieApi; fi'
                    //sh 'docker stop movieApi'
					sh 'docker rm movieApi'
					sh 'docker run --name movieApi -d -p 3000:3000 cristiancristancho/rampup_back:latest'
                                 					
				}                 
			} */         
		} 
} 
