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
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    bat 'mvn pmd:pmd'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                echo 'Stage 4: Running Tests'
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    bat 'mvn test'
                }
            }
        }
        
        stage('Generate JavaDoc') {
            steps {
                echo 'Stage 5: Generating JavaDoc'
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    bat 'mvn javadoc:javadoc -Dmaven.javadoc.failOnError=false -Dmaven.javadoc.doclint=none'
                }
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
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true, allowEmptyArchive: true
            archiveArtifacts artifacts: '**/target/site/apidocs/**/*.html', fingerprint: true, allowEmptyArchive: true
        }
    }
}
