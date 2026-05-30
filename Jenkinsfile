pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Stage 1: Checking out source code'
                checkout scm
            }
        }
        
        stage('Maven Build') {
            steps {
                echo 'Stage 2: Building with Maven'
                bat 'mvn clean install -DskipTests'
            }
        }
        
        stage('PMD Code Analysis') {
            steps {
                echo 'Stage 3: Running PMD Code Analysis'
                bat 'mvn pmd:pmd'
            }
        }
        
        stage('Run Tests') {
            steps {
                echo 'Stage 4: Running Tests'
                bat 'mvn test || echo "Tests failed but continuing..."'
            }
        }
        
        stage('Generate JavaDoc') {
            steps {
                echo 'Stage 5: Generating JavaDoc'
                bat 'mvn javadoc:javadoc -Dmaven.javadoc.failOnError=false -Dmaven.javadoc.doclint=none'
            }
        }
        
        stage('Package Application') {
            steps {
                echo 'Stage 6: Packaging Application'
                bat 'mvn package -DskipTests'
            }
        }
    }
    
    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true, allowEmptyArchive: true
        }
    }
}
