pipeline{
	stages{
		stage('Compilar') {
			node(label:'master') {
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'ab19b42c-1225-4cea-8ed3-211945c614df', url: 'https://github.com/n0rf3n/maven-project.git']]])
				sh  'mvn clean package'
				archiveArtifacts artifacts: '**/*.war', onlyIfSuccessful: true
			}
		}
		stage('CheckStyle') {
			node(label:'master') {
				sh returnStdout: true, script: 'mvn checkstyle:checkstyle'
				checkstyle canComputeNew: false, canRunOnFailed: true, defaultEncoding: '', failedTotalAll: params.MAX_ERRORS, healthy: '', pattern: '', unHealthy: ''
			}
		}
		stage('Desplegar') {
			node(label:'Windows') {
			copyArtifacts filter: '**/*.war', fingerprintArtifacts: true, flatten: true, projectName: 'pipelineDemo', selector: specific('$BUILD_NUMBER'), target: '$TOMCAT_HOME'
			}
		}
	}
}
