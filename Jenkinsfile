pipeline {
	agent any
	parameters {
		string(name: 'ha_version', defaultValue: '0.99.3', description: 'Home Assistant Version to Build')
	}
	stages {
		stage('Build Container') {
			steps {
				
				/* Pull Home Assistant Sources */
				echo "Checking out Version ${params.ha_version} from https://github.com/home-assistant/home-assistant"
				checkout changelog: false, poll: false, 
					scm: [$class: 'GitSCM', 
						branches: [[name: "refs/tags/${params.ha_version}"]], 
						doGenerateSubmoduleConfigurations: false, 
						extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '_source']],
						submoduleCfg: [], 
						userRemoteConfigs: [[
							refspec: '+refs/tags/*:refs/remotes/origin/tags/*', 
							url: 'https://github.com/home-assistant/home-assistant']]]
				
				/* Build Home Assistant using custom Dockerfile */
				dir('_source') {
					withDockerContainer(args: '-v /var/run/docker.sock:/run/docker.sock:rw -v ${pwd()}::/homeassistant:ro', image: 'homeassistant/amd64-builder') {
						sh '/builder.sh -h'
					}
				}
			}
		}
	}
}