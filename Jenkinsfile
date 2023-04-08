pipeline {
    agent any
    
    tools {
        maven 'Maven3.8.3'
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    // 使用maven步骤执行Maven命令
                    withMaven(
                        maven: 'Maven3.8.3',
                        mavenSettingsConfig: 'my-maven-settings',
                        mavenLocalRepo: '/root/.m2/repository'
                    ) {
                        sh 'mvn clean package'
                    }
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    // 使用maven步骤执行Maven命令
                    withMaven(
                        maven: 'Maven3.8.3',
                        mavenSettingsConfig: 'my-maven-settings',
                        mavenLocalRepo: '/root/.m2/repository'
                    ) {
                        sh 'mvn test'
                    }
                    
                    // 在post阶段中将测试结果转换为JSON并将其发送到后端
                    post {
                        always {
                            script {
                                def testResults = junit testResults: '**/target/surefire-reports/*.xml'
                                def json = new groovy.json.JsonBuilder(testResults).toPrettyString()
                                writeFile file: 'test-results.json', text: json
                                httpRequest url: 'http://192.168.6.88:8080/result', contentType: 'APPLICATION_JSON', requestBody: json
                            }
                        }
                    }
                }
            }
        }
    }
}
