pipeline {
	agent any
	parameters {
		string(name: 'ha_version', defaultValue: '0.98.5', description: 'Home Assistant Version to Build')
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
					HA_SRC_DIR = pwd()
					sh """
						docker run --rm --privileged \
							-v ~/.docker:/root/.docker:rw \
							-v /var/run/docker.sock:/run/docker.sock:rw \
							-v {HA_SRC_DIR}::/homeassistant:ro \
							homeassistant/amd64-builder \
								--homeassistant {params.ha_version} \
								--amd64 \
								-r https://github.com/asymworks/docker \
								-t home-assistant \
								--docker-hub asymworks \
								--test"""

				}
			}
		}
	}
}