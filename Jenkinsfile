pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            
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
