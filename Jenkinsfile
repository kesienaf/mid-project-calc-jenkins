pipeline {
    agent none
    stages {
        stage('Install') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Installations'
                sh 'git pull /home/centos/mid-project-calculator/'
                sh 'ansible-playbook /home/centos/mid-project-calculator/01-install.yml -i /home/centos/mid-project-calculator/hosts.ini'
            }
        }
        stage('Build') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Building the application'
                // Define build steps here
                sh 'ansible-playbook /home/centos/mid-project-calculator/03-build.yml -i /home/centos/mid-project-calculator/hosts.ini'
            }
        }
        stage('Test') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Running tests'
                // Define test steps here
                sh 'ansible-playbook /home/centos/mid-project-calculator/04-test.yml -i /home/centos/mid-project-calculator/hosts.ini'
                script {
                    echo 'Checking if files exist before stashing'
                }
                stash(allowEmpty: true, name: 'Jenkins-Mid-Project-Calc', includes: "/path/to/checkout/target/*.war")
            }
        }
        stage('Deploy') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Deploying the application'
                // Define deployment steps here
                unstash 'Jenkins-Mid-Project-Calc'
                sh "sudo rm -rf ~/opt/tomcat/webapp/*.war" 
                sh "sudo mv /path/to/checkout/target/*.war ~/opt/tomcat/webapps/"
                sh 'ansible-playbook /home/centos/mid-project-calculator/07-deploy.yml -i /home/centos/mid-project-calculator/hosts.ini'
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
