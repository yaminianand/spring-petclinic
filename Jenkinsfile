pipeline{
    //agent{label 'master'}
    agent{label 'slave1'}    
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
                    
		sh "sudo docker build . -t anjurose/petclinic"
		sh "sudo docker run -d -p 8091:8080 anjurose/petclinic"
                //ansiblePlaybook credentialsId: 'ubuntu', disableHostKeyChecking: true, inventory: '/etc/ansible/hosts', playbook: './petclinic_latest.yml' 
                //sh "sudo /opt/puppetlabs/bin/puppet agent -t"   
            }
        }
    }
}

