pipeline {
	agent any
	stages {
		stage('Clone sources') {
			steps {				
				git url: 'https://github.com/emc-mongoose/mongoose.git'
			}
		}
		stage('Unit tests and results') {
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
	agent {
        docker { image 'node:7-alpine' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}