pipeline {
    agent none
    stages {
        stage('Install') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Installations'
                sh 'ansible-playbook 01-install.yml -i hosts.ini'
            }
        }
        stage('Build') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Building the application'
                // Define build steps here
                sh 'ansible-playbook 03-build.yml -i hosts.ini'
            }
        }
        stage('Test') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Running tests'
                // Define test steps here
                sh 'ansible-playbook 04-test.yml -i hosts.ini'
                stash (name: 'Jenkins-Mid-Project', includes: "target/*.war")
            }
        }
        stage('Deploy') {
            agent {
                label 'node-1'
            }
            steps {
                echo 'Deploying the application'
                // Define deployment steps here
                unstash 'Jenkins-Mid-Project'
                sh 'ansible-playbook 05-deploy.yml -i hosts.ini'
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
