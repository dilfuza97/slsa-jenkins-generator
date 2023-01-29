def artifact_path
def artifact_name

pipeline {
    agent any
//     tools {
//         git "git"
//         go 'go 1.18'

//     }

    environment {
        CREDENTIAL_ID = '52df5492-8277-4db2-b0bc-fd85aa25b9f2'
        Repository_Generator = 'https://github.com/Samsung/slsa-jenkins-generator'
	payload = '{	"actor": "",	"event_payload": "",	"build_url": "test",	"payload_repository_git_url": "prgu",	"payload_after": "1234",	"eventname": "somename"}'
    }


	
	
    stages {
        stage('build target ') {
            steps {
                script {
                    git branch: "main", credentialsId: "$CREDENTIAL_ID", url: "https://github.com/dilfuza97/slsa-jenkins-generator.git"

                     //TODO: replace with real build command
//                    sh 'echo "hello world" > output.txt'
                    sh """
                    cat << EOF > output.json
{
	"actor": "",
	"event_payload": "",
	"build_url": "test",
	"payload_repository_git_url": "prgu",
	"payload_after": "1234",
	"eventname": "somename"
}
EOF
                    """
                    sh "cat output.json"
                    artifact_path = sh(script: 'pwd', returnStdout: true).trim()
                    artifact_name = "output.json"
                }
            }
        }

        stage('generate provenance') {
            steps {
              sh "pwd & ls -ahl"
                
                        // 'run generator via docker'
                        echo 'run generator via docker'
                        sh "docker build . -t scia:slsa-generator"
                        sh "printenv > ./envlist && docker run --env-file ./envlist -v \"${artifact_path}\":\"/artifacts\" scia:slsa-generator -a artifacts/${artifact_name} -o artifacts"
                        sh "cat output.json"
                
            }
        }
    }
}
