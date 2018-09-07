pipeline {
	agent any
	stages {
		stage('Clone sources') {
			steps {
				cleanWs()
				git url: 'https://github.com/emc-mongoose/mongoose.git'
			}
		}
		stage('Build and archive artifacts') {
			steps {
				sh "./gradlew clean dist"
				archiveArtifacts artifacts: 'build/dist/*.tgz', fingerprint: true
			}
		}
		stage('Test and report') {
			steps {
				sh "./gradlew :tests:unit:test"
				step $class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: '**/unit/build/test-results/test/TEST-*.xml'
			}
		}
	}
}