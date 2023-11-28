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
                        playbook: '/home/centos/mid-project-calculator/01-install.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
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
                        playbook: '/home/centos/mid-project-calculator/02-build.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
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
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/03-test.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
                        )
                }
            }
        }
        stage('Install and Start Tomcat') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Install and Start Tomcat'
                script {
                    // Installing Tomcat using ansible playbook
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/04-install-tomcat.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
                    )
                }
            }
        }
        stage('Remove War Files from Ansible Master') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Remove War Files'
                script {
                    // Remove Previous WAR files from the Ansible Master using ansible playbook
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/05-remove-war-file.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
                    )
                }
            }
        }
        stage('Copy War Files') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Copy'
                script {
                    // DCopy new WAR Files using ansible playbook
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/06-copy-war-files.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
                    )
                }
            }
        }
        stage('Paste War Files') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Paste'
                script {
                    // Deploying the application using ansible playbook
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/07-paste-war-files.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
                    )
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
                    // Deploying the application using ansible playbook
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/08-deploy-application.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
                    )
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
