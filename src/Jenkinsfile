pipeline {
    agent any
    
    triggers {
        cron('H/10 * * * 1')  // Run every 10 minutes on Mondays
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }
        
        stage('Coverage') {
            steps {
                sh './mvnw org.jacoco:jacoco-maven-plugin:report'
            }
            post {
                always {
                    publishCoverage(
                        adapters: [jacocoAdapter('target/site/jacoco/jacoco.xml')]
                    )
                }
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
