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
    }

    stages {
        stage('build target ') {
            steps {
                script {
                    git branch: "main", credentialsId: "$CREDENTIAL_ID", url: "https://github.com/slsa-framework/slsa-jenkins-generator.git"

                     //TODO: replace with real build command
                    sh 'echo "hello world" > output.txt'
                    artifact_path = sh(script: 'pwd', returnStdout: true).trim()
                    artifact_name = "output.txt"
                }
            }
        }

        stage('generate provenance') {
            steps {
                dir("../workspace") {
                    sh 'rm -rf slsa-jenkins-generator && mkdir slsa-jenkins-generator'

                    dir("slsa-jenkins-generator") {
                        git branch: "main", credentialsId: "$CREDENTIAL_ID", url: "$Repository_Generator"

                        // 'run generator via docker'
                        echo 'run generator via docker'
                        sh "docker build . -t scia:slsa-generator"
                        sh "printenv > ./envlist && docker run --env-file ./envlist -v \"${artifact_path}\":\"/artifacts\" scia:slsa-generator -a artifacts/${artifact_name} -o artifacts"
                    }
                }
            }
        }
    }
}
