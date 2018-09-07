pipeline {	
	agent any
	stages {
		stage('Clone sources') {
			//agent any
			steps {				
				git url: 'https://github.com/emc-mongoose/mongoose.git'
			}			
		}
		stage('Build') {
			//agent any
			steps {
				println pwd()
				//sh "'${tool 'Gradle 3.5'}/bin/gradle' clean build"
				sh "./gradlew clean dist"
			}			
		}
		stage('Test and report') {
			//agent any
			steps {
				//sh "'${tool 'Gradle 3.5'}/bin/gradle' :tests:unit:test"
				sh "./gradlew :tests:unit:test"
				step $class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: '**/unit/build/test-results/test/TEST-*.xml'
			}			
		}
		stage('Archive artifacts') {
			steps {
				archiveArtifacts artifacts: 'build/dist/*.tgz', fingerprint: true
			}
		}
	}
}