pipeline {
    agent any

    stages {
        stage('Installing Docker') {
            steps {
                sh '''
                sudo yum install docker -y
                sudo service docker start
                sudo usermod -aG docker $USER
                sudo usermod -aG docker jenkins
                sudo chmod 666 /var/run/docker.sock
                docker images
                '''
            }
        }
        stage('Installing Trivy') {
            steps {
                sh '''
                curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sudo sh -s -- -b /usr/local/bin
                trivy --version
                '''
            }
        }
        stage('Pulling Docker Image from my DockerHub') {
            steps {
                sh '''
                echo "Pulling Docker image..."
                docker pull muralikaspa1998/website:one
                '''
            }
        }
        stage('Scanning Docker image') {
            steps {
                sh '''
                echo "Scanning Docker Image by using Trivy"
                trivy image muralikaspa1998/website:one
                echo "Creating the output of this Vulnerability test into a File"
                trivy image muralikaspa1998/website:one > ImageScanReport.txt
                '''
            }
        }
        stage('Installing SonarQube') {
            steps {
                sh '''
                echo "Downloading and installing SonarQube Scanner..."
                wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-6.2.1.4610-linux-x64.zip
                unzip -o sonar-scanner-cli-6.2.1.4610-linux-x64.zip -d /tmp/sonar-scanner
                sonar-scanner --version
                '''
            }
        }
    }
}

