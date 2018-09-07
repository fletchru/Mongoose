pipeline {
	agent any
	stages {
		stage('Clone sources') {
			steps {
				cleanWs()
				git url: 'https://github.com/emc-mongoose/mongoose.git'
			}
		}
		stage('Test and report') {
			steps {
				sh "./gradlew clean :tests:unit:test"
				step $class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: '**/unit/build/test-results/test/TEST-*.xml'
			}
		}
		stage('Build and archive artifacts') {
			steps {
				sh "./gradlew dist"
				archiveArtifacts artifacts: 'build/dist/*.tgz', fingerprint: true
			}
		}		
	}
}