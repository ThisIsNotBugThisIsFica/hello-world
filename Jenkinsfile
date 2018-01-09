node('test-slave') {
		stage('Checkout'){
				checkout scm
		}

		stage('build'){
				sh 'make'
		}

		stage ('Artifactory configuration'){
				def server = Artifactory.newServer url: 'http://localhost:8081/artifactory', credentialsId: 'artifactory_user'

				// Read the upload spec which was downloaded from github.
				def uploadSpec = readFile 'hello-world/upload.json'
				// Upload to Artifactory.
				def buildInfo = server.upload spec: uploadSpec
				buildInfo.env.capture = true
		}

		stage ('Publish build info'){
				server.publishBuildInfo buildInfo
		}
}
