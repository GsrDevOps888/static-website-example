#!/groovy
def dockerImageRepo = 'anandtest/protest'
def dockerImageTag
def dockerImage
def dockerRegistry = 'hub.docker.com'

pipeline
{
	agent any
	stages
	{
		stage('Cleaning the WorkSpace')
		{
			steps
			{
				deleteDir()
				echo "the build number is ${currentBuild.number}"
				echo 'Cleanup Done'
			}
		}
		
		stage('CheckOut latest Code')
		{
			steps
			{
				checkout scm
				script 
				{
					dockerImageTag="$BUILD_NUMBER"
				}
			}
		}
		
		stage('Build the Image')
		{
			steps
			{
				script 
				{
					echo 'Starting the Image Building'
					dockerImage = docker.build "${dockerImageRepo}"
					sh 'docker images'
					sh 'docker ps -a'
					echo "$dockerImage"
				}
			}
		}
		
		stage('Publish Docker Images to DockerHub')
		{
			steps
			{
				echo "Pushing Docker image to Registory"
				script
				{
					sh 'docker login --username="anandgit71" --password="anandgit12" ${dockerRegistry}'
					dockerImage.push()
					// sh 'docker rmi $(docker images -a -q)'
					// sh 'docker images'
				}
			}
		}
		
		stage('Remote Deployment')
		{
			steps
			{
				script
				{
					sh '''ansible-playbook remote-deploy.yml --key-file /tmp/private.pem'''
				}
			}
		}
	}
}
