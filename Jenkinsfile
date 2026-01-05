pipeline {
    agent any

    parameters {
        string(
            name: 'GIT_BRANCH',
            defaultValue: 'main',
            description: 'Enter Git branch name (e.g. main, dev, feature-1)'
        )

        choice(
            name: 'ACTION',
            choices: ['plan', 'apply'],
            description: 'Select the Terraform action'
        )
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: "*/${params.GIT_BRANCH}"]],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Prathamesh1002/Terraform-Automation.git'
                    ]]
                )
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init -reconfigure'
            }
        }

        stage('Action') {
            steps {
                script {
                    switch (params.ACTION) {
                        case 'plan':
                            echo "Running Terraform PLAN on branch: ${params.GIT_BRANCH}"
                            sh 'terraform plan'
                            break

                        case 'apply':
                            echo "Running Terraform APPLY on branch: ${params.GIT_BRANCH}"
                            sh 'terraform apply -auto-approve'
                            break

                        default:
                            error 'Unknown action'
                    }
                }
            }
        }
    }
}
