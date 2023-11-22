pipeline {
    agent none
    stages {
        stage('Install') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Installations'
                script {
                    sh 'git pull /home/centos/mid-project-calculator/'
                    
                    // Install all modules using ansible playbook
                    ansiblePlaybook(
                        playbook: '01-install.yml"
                        inventory: "hosts.ini"
                    )
                }
            }
        }
        stage('Build') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Build'
                script {
                    // Building the application using ansible playbook
                    ansiblePlaybook(
                        playbook: '03-build.yml"
                        inventory: "hosts.ini"
                    )
                }
            }
        }
        stage('Test') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Test'
                script {
                    // Testing the application using ansible playbook
                    ansiblePlaybook{
                        playbook: '04-test.yml"
                        inventory: "hosts.ini"
                        )

                    //Stash war files
                    stash (name: 'mid-project-calculator', includes: "target/*.war")
                    }
                }
            }
        stage('Deploy') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Deploying the application'
                script {
                    //Unstash files
                    unstash 'mid-project-calculator'
                    
                    // Deploying the application using ansible playbook
                    ansiblePlaybook{
                        disableHostKeyChecking: true,
                        playbook: '07-deploy.yml"
                        inventory: "hosts.ini"
                    }
                }
            }
        }
    }
    post {
        success {
            script {
                // Send email for successful build
                mail to: 'kesienafels@gmail.com',
                     subject: "Build Successful - ${currentBuild.fullDisplayName}",
                     body: "Congratulations! The build was successful.\n\nCheck console output at ${BUILD_URL}"
            }
        }
        failure {
            script {
                // Send email for failed build
                mail to: 'kesienafels@gmail.com',
                     subject: "Build Failed - ${currentBuild.fullDisplayName}",
                     body: "Oops! The build failed.\n\nCheck console output at ${BUILD_URL}"
            }
        }
    }
}
