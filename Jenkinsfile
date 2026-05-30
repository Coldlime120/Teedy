pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                echo '========================================='
                echo 'Stage 1: Checking out source code'
                echo '========================================='
                checkout scm
            }
        }
        
        stage('Maven Build') {
            steps {
                echo '========================================='
                echo 'Stage 2: Building with Maven'
                echo '========================================='
                bat 'mvn clean install -DskipTests'
            }
        }
        
        stage('PMD Code Analysis') {
            steps {
                echo '========================================='
                echo 'Stage 3: Running PMD Code Analysis'
                echo '========================================='
                bat 'mvn pmd:pmd'
            }
        }
        
        stage('Run Tests') {
            steps {
                echo '========================================='
                echo 'Stage 4: Running Unit Tests'
                echo '========================================='
                bat 'mvn test'
            }
        }
        
        stage('Generate Test Reports') {
            steps {
                echo '========================================='
                echo 'Stage 5: Generating Test Reports'
                echo '========================================='
                bat 'mvn surefire-report:report'
            }
        }
        
        stage('Generate JavaDoc') {
            steps {
                echo '========================================='
                echo 'Stage 6: Generating JavaDoc Documentation'
                echo '========================================='
                bat 'mvn javadoc:javadoc -Dshow=private'
            }
        }
        
        stage('Package Application') {
            steps {
                echo '========================================='
                echo 'Stage 7: Packaging Application'
                echo '========================================='
                bat 'mvn package -DskipTests'
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
