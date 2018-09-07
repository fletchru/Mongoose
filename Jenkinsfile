pipeline {
	agent none
	stages {
		stage('Clone sources') {
			agent any
			git url: 'https://github.com/emc-mongoose/mongoose.git'
		}

		stage('Build') {
			agent any
			sh "'${tool 'Gradle 3.5'}/bin/gradle' clean build"
		}

		stage('Test and report') {
			agent any
			sh "'${tool 'Gradle 3.5'}/bin/gradle' :tests:unit:test"
			step $class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: '**/unit/build/test-results/test/TEST-*.xml'
		}

		stage('Archive artifacts') {
			agent any
			def files = findFiles(glob: 'build/libs/mongoose-*.jar')
			files += findFiles(glob: '**/mongoose-storage-driver-service-*.jar')

			files.each {
				def archiveFileName = "${it.name.replace("jar", "tgz")}"
				sh "tar -cvzf " + archiveFileName + " ${it.path}"
				archiveArtifacts artifacts: archiveFileName, fingerprint: true
			}
		}

	}
}