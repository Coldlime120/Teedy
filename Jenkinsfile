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
        
        // 阶段4：运行测试（先生成测试结果）
        stage('Run Tests') {
            steps {
                echo '========================================='
                echo 'Stage 4: Running Tests'
                echo '========================================='
                bat 'mvn test'
            }
        }
        
        // 阶段5：生成测试报告（修正：分开执行）
        stage('Generate Test Reports') {
            steps {
                echo '========================================='
                echo 'Stage 5: Generating Test Reports'
                echo '========================================='
                bat 'mvn surefire-report:report -pl docs-core,docs-web-commons,docs-web'
            }
        }
        
        // 阶段6：生成 JavaDoc（修正：针对所有模块）
        stage('Generate JavaDoc') {
            steps {
                echo '========================================='
                echo 'Stage 6: Generating JavaDoc Documentation'
                echo '========================================='
                bat 'mvn javadoc:javadoc -Dmaven.javadoc.failOnError=false -Dmaven.javadoc.doclint=none -pl docs-core,docs-web-commons,docs-web'
            }
        }
        
        // 阶段7：打包应用
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
        always {
            echo '========================================='
            echo 'Post-build: Archiving artifacts'
            echo '========================================='
            
            // 归档 JAR 文件（多个模块可能有多个 JAR）
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true, allowEmptyArchive: true
            archiveArtifacts artifacts: '**/docs-*/target/*.jar', fingerprint: true, allowEmptyArchive: true
            
            // 归档测试结果 XML（使用正确的路径模式）
            archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', fingerprint: true, allowEmptyArchive: true
            archiveArtifacts artifacts: '**/docs-*/target/surefire-reports/*.xml', fingerprint: true, allowEmptyArchive: true
            
            // 发布测试结果
            junit '**/target/surefire-reports/*.xml'
            junit '**/docs-*/target/surefire-reports/*.xml'
            
            // 归档 PMD 报告（如果生成的话）
            archiveArtifacts artifacts: '**/target/site/pmd.html', fingerprint: true, allowEmptyArchive: true
        }
        
        success {
            echo '========================================='
            echo '✓ Build completed successfully!'
            echo '✓ All artifacts have been archived'
            echo '========================================='
        }
        
        unstable {
            echo '========================================='
            echo '⚠ Build completed but with warnings!'
            echo '⚠ Please check the artifact archives'
            echo '========================================='
        }
        
        failure {
            echo '========================================='
            echo '✗ Build failed. Please check the logs.'
            echo '========================================='
        }
    }
}
