
pipeline {
    agent any
	environment {
		ARTIFACTID = readMavenPom().getArtifactId()
		VERSION = readMavenPom().getVersion()
		PACKAGING = readMavenPom().getPackaging()
		GROUPID = readMavenPom().getGroupId()
		
	}
	
    stages {

		stage('SCM') {
            steps {
				git 'https://github.com/shivanshkr/jenkins.git'
            }
        }
        stage('Build') {
            steps {
				echo 'Building..'
				withMaven( ) {
						bat "mvn clean compile"
				} 
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
				withMaven( ) {
						bat "mvn test "
				}
            }
        }
		 stage('Package') {
            steps {
                echo 'Packaging..'
				withMaven( maven: 'apache-maven-3.6.2') {
						bat "mvn package "
				}
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying.... '
				nexusArtifactUploader ( 
					artifacts: [
						[artifactId: "${ARTIFACTID}", 
						classifier: '', 
						file: 'target\\maven-webapp.war', 
						type: "${PACKAGING}" ]
					], 
					credentialsId: '1431b1bf-5b26-4599-ba2b-481b76097d07', 
					groupId: "${GROUPID}", 
					nexusUrl: 'localhost:8081', 
					nexusVersion: 'nexus3', 
					protocol: 'http', 
					repository: 'demo', 
					version: "${VERSION}" 
				)
            }
        }
		stage('Archive'){
			steps{
				echo 'Archiving.... '
				archiveArtifacts '**'
			}
		}
    }
	
}