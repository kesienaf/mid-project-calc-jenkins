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
                        playbook: '/home/centos/mid-project-calculator/03-build.yml',
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
                        playbook: '/home/centos/mid-project-calculator/04-test.yml',
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
                    // Deploying the application using ansible playbook
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/05-install-tomcat.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
                    )
                }
            }
        }
        stage('Fetch War Files') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Fetch'
                script {
                    // Deploying the application using ansible playbook
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/06-fetch.yml',
                        inventory: '/home/centos/mid-project-calculator/hosts.ini'
                    )
                }
            }
        }
        stage('Copy and Paste War Files') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Copy and Paste'
                script {
                    // Deploying the application using ansible playbook
                    ansiblePlaybook(
                        playbook: '/home/centos/mid-project-calculator/07-copy-paste.yml',
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
