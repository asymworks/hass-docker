pipeline {
	agent any
	parameters {
		string(name: 'ha_version', defaultValue: '0.98.5', description: 'Home Assistant Version to Build')
	}
	stages {
		stage('Build Container') {
			steps {
				echo "Checking out tags/${params.ha_version} from https://github.com/home-assistant/home-assistant"
				git branch: "tags/${params.ha_version}", changelog: false, poll: false, url: 'https://github.com/home-assistant/home-assistant'
			}
		}
	}
}