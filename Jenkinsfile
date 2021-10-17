pipeline {
    agent {
        label "slave"
    }

    tools{
        nodejs "uglify-js & clean-css-cli"
    }

    stages {
        stage("SCM_Checkout"){
            steps{
             git url: "https://github.com/W1ckedS1ck/material-design-template"
             sh "git --version"
             sh "npm list -g"
            }
        }
        stage("Compress"){
            parallel {
                stage("uglifyjs"){
                    steps{
                        sh 'cat www/js/* | uglifyjs -o www/min/merged-and-compressed.js --compress'
                    }
                }
                stage("cleancss"){
                    steps{
                        sh 'cat www/css/* | cleancss -o www/min/merged-and-minified.css'
                    }
                }
            }
        }
        stage ('pack'){
			steps{
				sh "tar --exclude=.git --exclude=www/css --exclude=www/js -czvf arc.tar.gz *" 
			}
		}
    }
    post{
		success {
			archiveArtifacts artifacts: 'arc.tar.gz', fingerprint: true
			echo "We're good so far"
		}
		always {
			echo "${BUILD_ID}"
		}  
	}
}
