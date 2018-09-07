pipeline {	
	agent any
	stages {
		stage('Clone sources') {
			//agent any
			steps {				
				git url: 'https://github.com/emc-mongoose/mongoose.git'
			}			
		}
		stage('Test and report') {
			//agent any
			steps {				
				sh "./gradlew :tests:unit:test"
				step $class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: '**/unit/build/test-results/test/TEST-*.xml'
			}			
		}
		stage('Build and archive artifacts') {
			//agent any
			steps {
				sh "./gradlew clean dist"
				archiveArtifacts artifacts: 'build/dist/*.tgz', fingerprint: true
			}
		}
	}
}