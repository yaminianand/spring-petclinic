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
                input 'Do you approve the deployment?'
                sh 'scp target/*.jar produser@45.76.96.139:/home/produser/'
            	sh "ssh produser@45.76.96.139 'nohup java -jar /home/produser/spring-petclinic-2.1.0.BUILD-SNAPSHOT.jar &'"
		}
        }
    }
}

