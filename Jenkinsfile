pipeline {
    agent any
    
    tools {
        maven 'Maven-3.8.6'
        jdk 'JDK-11'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                echo '========================================='
                echo 'Stage 1: Checking out source code'
                echo '========================================='
                checkout scm
                script {
                    def gitCommit = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
                    echo "Commit: ${gitCommit}"
                }
            }
        }
        
        stage('Maven Build') {
            steps {
                echo '========================================='
                echo 'Stage 2: Building with Maven'
                echo '========================================='
                sh 'mvn clean compile -DskipTests'
            }
        }
        
        stage('PMD Code Analysis') {
            steps {
                echo '========================================='
                echo 'Stage 3: Running PMD Code Analysis'
                echo '========================================='
                sh 'mvn pmd:pmd'
            }
        }
        
        stage('Run Tests') {
            steps {
                echo '========================================='
                echo 'Stage 4: Running Unit Tests'
                echo '========================================='
                sh 'mvn test'
            }
        }
        
        stage('Generate Test Reports') {
            steps {
                echo '========================================='
                echo 'Stage 5: Generating Test Reports'
                echo '========================================='
                sh 'mvn surefire-report:report'
            }
        }
        
        stage('Generate JavaDoc') {
            steps {
                echo '========================================='
                echo 'Stage 6: Generating JavaDoc Documentation'
                echo '========================================='
                sh 'mvn javadoc:javadoc -Dshow=private'
            }
        }
        
        stage('Package Application') {
            steps {
                echo '========================================='
                echo 'Stage 7: Packaging Application'
                echo '========================================='
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: '**/target/*.war,**/target/*.jar', fingerprint: true
            }
        }
    }
    
    post {
        success {
            echo '========================================='
            echo 'Build completed successfully!'
            echo '========================================='
        }
        failure {
            echo '========================================='
            echo 'Build failed. Please check the logs.'
            echo '========================================='
        }
    }
}
