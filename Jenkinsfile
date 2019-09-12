pipeline {
	agent any
	parameters {
		string(name: 'ha_version', defaultValue: '0.98.5', description: 'Home Assistant Version to Build')
	}
	stages {
		stage('Build Container') {
			steps {
				sh 'pwd'
				sh 'ls -l'

				/* Pull Home Assistant Sources */
				echo "Checking out Version ${params.ha_version} from https://github.com/home-assistant/home-assistant"
				checkout changelog: false, poll: false, 
					scm: [$class: 'GitSCM', 
						branches: [[name: "refs/tags/${params.ha_version}"]], 
						doGenerateSubmoduleConfigurations: false, 
						extensions: [], 
						submoduleCfg: [], 
						userRemoteConfigs: [[
							refspec: '+refs/tags/*:refs/remotes/origin/tags/*', 
							url: 'https://github.com/home-assistant/home-assistant']]]
				
				/* Build Home Assistant using custom Dockerfile */
				sh 'pwd'
				sh 'ls -l'
			}
		}
	}
}