pipeline {
    agent any
    
    environment {
        MAVEN_OPTS = "-Xmx512m"
    }
    
    stages {
        // 阶段1：代码检出
        stage('Checkout Code') {
            steps {
                echo '========================================='
                echo 'Stage 1: Checking out source code'
                echo '========================================='
                checkout scm
            }
        }
        
        // 阶段2：Maven 构建
        stage('Maven Build') {
            steps {
                echo '========================================='
                echo 'Stage 2: Building with Maven'
                echo '========================================='
                bat 'mvn clean install -DskipTests'
            }
        }
        
        // 阶段3：PMD 代码检查
        stage('PMD Code Analysis') {
            steps {
                echo '========================================='
                echo 'Stage 3: Running PMD Code Analysis'
                echo '========================================='
                bat 'mvn pmd:pmd'
            }
        }
        
        // 阶段4：生成测试报告
        stage('Generate Test Reports') {
            steps {
                echo '========================================='
                echo 'Stage 4: Generating Test Reports'
                echo '========================================='
                // 注意：这里不再跳过测试
                bat 'mvn surefire-report:report'
            }
        }
        
        // 阶段5：生成 JavaDoc（修复版本）
        stage('Generate JavaDoc') {
            steps {
                echo '========================================='
                echo 'Stage 5: Generating JavaDoc Documentation'
                echo '========================================='
                // 添加 JDK 21 兼容性参数
                bat 'mvn javadoc:javadoc -Dmaven.javadoc.failOnError=false -Dmaven.javadoc.doclint=none'
            }
        }
        
        // 阶段6：打包应用
        stage('Package Application') {
            steps {
                echo '========================================='
                echo 'Stage 6: Packaging Application'
                echo '========================================='
                bat 'mvn package -DskipTests'
            }
        }
    }
    
    post {
        always {
            echo '========================================='
            echo 'Post-build: Archiving artifacts'
            echo '========================================='
            
            // 归档 JAR 文件
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true, allowEmptyArchive: true
            
            // 归档测试报告
            archiveArtifacts artifacts: '**/target/site/surefire-report.html', fingerprint: true, allowEmptyArchive: true
            archiveArtifacts artifacts: '**/docs-core/target/site/surefire-report.html', fingerprint: true, allowEmptyArchive: true
            
            // 归档 JavaDoc
            archiveArtifacts artifacts: '**/target/site/apidocs/**/*.html', fingerprint: true, allowEmptyArchive: true
            archiveArtifacts artifacts: '**/docs-core/target/site/apidocs/**/*.html', fingerprint: true, allowEmptyArchive: true
            
            // 发布测试结果
            junit '**/target/surefire-reports/*.xml'
            junit '**/docs-core/target/surefire-reports/*.xml'
        }
        
        success {
            echo '========================================='
            echo '✓ Build completed successfully!'
            echo '✓ All artifacts have been archived'
            echo '========================================='
        }
        
        failure {
            echo '========================================='
            echo '✗ Build failed. Please check the logs.'
            echo '========================================='
        }
    }
}
