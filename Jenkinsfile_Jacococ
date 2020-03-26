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
        stage('verify'){
            steps{
	    		sh 'mvn clean verify'
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
        	publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site/jacoco', reportFiles: 'index.html', reportName: 'Code Coverage from HTML Publisher', reportTitles: ''])
		jacoco exclusionPattern: '**/src/main/java', inclusionPattern: '**/classes'
		}
        }
        stage('Deploy'){
            steps{
		sh "docker build . -t anjurose/petclinic"
		sh "docker run -d -p 8087:8080 anjurose/petclinic"
            }
        }
    }
}

