node(env.SLAVE) {
	def student = 'pyurchuk'
	checkout([$class: 'GitSCM', branches: [[name: '*/pyurchuk']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MNT-Lab/mntlab-pipeline.git']]])
	env.PATH=env.PATH+":/opt/gradle-4.0.1/bin:/opt/groovy-2.4.12/bin"

stage('Preparation (Checking out)') { 
	git([url: 'https://github.com/MNT-Lab/mntlab-pipeline.git', branch: "${student}"])
	}

stage('Builing code') {
	sh "gradle build"  
	}
    
stage('Testing') {
parallel (
    'Cucumber Tests': { sh "gradle cucumber" },
    'Jacoco Tests': { sh "gradle jacocoTestReport" },
    'Unit Tests': { sh "gradle test" }
    )
	}

stage ('Triggering job and fetching artefact after finishing') {
build job: "EPBYMINW6405/MNTLAB-pyurchuk-child1-build-job", parameters: [string(name: 'BRANCH_NAME', value: 'pyurchuk')]
echo WORKSPACE

step(
    [$class: 'CopyArtifact',
    filter: "pyurchuk_dsl_script.tar.gz",
    projectName: "MNTLAB-pyurchuk-child1-build-job" ])
	}

stage ('Packaging and Publishing results') {
    sh 'tar -xf pyurchuk_dsl_script.tar.gz jobs.groovy'
    sh 'tar -czf pipeline-pyurchuk-"${BUILD_NUMBER}".tar.gz jobs.groovy Jenkinsfile -C build/libs gradle-simple.jar'
    archiveArtifacts 'pipeline-pyurchuk-${BUILD_NUMBER}.tar.gz'
    sh 'groovy pull-push.groovy -p push -a pipeline-pyurchuk-${BUILD_NUMBER}.tar.gz'    
	}

stage ('Asking for manual approval') {
   timeout(time:1, unit:'HOURS') {
	input message:'Please approve current deployment', ok: 'Yes'
	 }
	}

stage ('Deployment') {
	sh 'java -jar build/libs/gradle-simple.jar'
	}

stage ('Sending status') {
	echo 'Status: SUCCESS!'
	}
}
