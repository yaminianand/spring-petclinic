pipeline{
    agent{label 'master'}
    tools{
        maven 'M3'
    }
    stages{
        stage('Checkout'){
            steps{
                git 'https://github.com/AnjuMeleth/spring-petclinic.git'
            }
        }
        stage('Build'){
            steps{
                 sh 'mvn clean compile'
            }
        }
        stage('Test'){
            steps{
                     sh 'mvn test'
                     junit '**/target/surefire-reports/TEST-*.xml'
            }
        }
        stage('Package'){
            steps{
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy'){
            steps{
		sh 'scp /var/lib/jenkins/workspace/SpringPetclinic/target/*.jar produser@45.76.96.139:/home/produser'
		sh 'scp /var/lib/jenkins/workspace/SpringPetclinic/Dockerfile produser@45.76.96.139:/home/produser'
		sh "ssh produser@45.76.96.139 'docker build /home/produser -t anjurose/petclinic'"
		sh "ssh produser@45.76.96.139 'docker run -d -p 8087:8080 anjurose/petclinic'"
            }
        }
    }
}

