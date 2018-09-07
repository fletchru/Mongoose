pipeline {	
	agent none
	stages {
		stage('Clone sources') {
			agent any
			steps {
				git url: 'https://github.com/emc-mongoose/mongoose.git'
			}			
		}
		stage('Build') {
			agent any
			steps {
				println pwd()
				sh "'${tool 'Gradle 3.5'}/bin/gradle' clean build"
				//sh "./gradlew clean build"
			}			
		}
		stage('Test and report') {
			agent any
			steps {
				sh "'${tool 'Gradle 3.5'}/bin/gradle' :tests:unit:test"
				step $class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: '**/unit/build/test-results/test/TEST-*.xml'
			}			
		}
		stage('Archive artifacts') {
			agent any
			environment {
				files = findFiles(glob: 'build/libs/mongoose-*.jar')				
			}
			steps {				
				script {
					files += findFiles(glob: '**/mongoose-storage-driver-service-*.jar')
					files.each {
						def archiveFileName = "${it.name.replace("jar", "tgz")}"
						sh "tar -cvzf " + archiveFileName + " ${it.path}"
						archiveArtifacts artifacts: archiveFileName, fingerprint: true
					}
				}
			}
		}
	}
}