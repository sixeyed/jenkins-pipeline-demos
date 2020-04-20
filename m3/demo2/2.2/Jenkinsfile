library identifier: 'jenkins-pipeline-demo-library@master', 
        retriever: modernSCM([$class: 'GitSCMSource', remote: 'https://github.com/sixeyed/jenkins-pipeline-demo-library.git'])

pipeline {
    agent any
    stages {
        stage('Audit tools') {                        
            steps {
                auditTools2 message: 'This is demo 2'
            }
        }
    }
}
