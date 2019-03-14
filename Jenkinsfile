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

			stage('Deploy') {                         
				steps {                                 
					echo 'Deploying....'  
					input 'Accept deployment?'
					//sh 'docker stop $(docker ps -aq)'
					// sh 'docker stop challjenkNew'
					// sh 'docker stop challjenk'
					// sh 'docker rm challjenk'
					// sh 'docker run --name challjenk -d -p 8000:3030 cristiancristancho/rampup_front:latest'
					//sh 'ssh -i cccc.pem ubuntu@172.23.6.170'
					//sh 'if [ "$(docker service ps -q movieAPI)" != "" ]; then docker service create --replicas 1 -p 3000:3000 --constraint node.hostname==ip-172-23-9-232 --name movieAPI cristiancristancho/rampup_back:latest; else docker service update --replicas 1 -p 3000:3000 --constraint node.hostname==ip-172-23-9-232 --name movieAPI cristiancristancho/rampup_back:latest; fi'
					sh 	'''
							ssh -v -i /var/lib/jenkins/cccc.pem ubuntu@172.23.6.170 "if [ "$(docker service ps -q movieAPI)" != "no such service: movieAPI" ]; then docker service create --replicas 1 -p 3000:3000 --constraint node.hostname==ip-172-23-9-232 --name movieAPI cristiancristancho/rampup_back:${BUILD_NUMBER}; else docker service update --image cristiancristancho/rampup_back:${BUILD_NUMBER} movieAPI; fi"
						'''
					// sh 'docker service update --replicas 1 -p 3000:3000 --name movieAPI cristiancristancho/rampup_back:latest'
				}                 
			}         
		} 
} 
