pipeline {   
         agent any
         triggers {
                  pollSCM('*/5 * * * *')
         }
         stages {
                  stage('Build') {                         
                           steps {
                                    echo 'Building...'
                                    sh 'mvn clean compile assembly:single'
                                    archiveArtifacts 'target/ownproject-1.0.0-jar-with-dependencies.jar'
                           }
                  }
                  stage('Test') {                         
                           steps {
                                    echo 'Testing & Emmaing...'
                                    sh 'mvn emma:emma'
                                    
                                    // jUnit speichern
                                    junit 'target/surefire-reports/*.xml'
                                    
                                    // Publish Emma as HTML Report
                                    publishHTML target: [
                                             allowMissing: true,
                                             alwaysLinkToLastBuild: false,
                                             keepAll: true,
                                             reportDir: 'target/site/emma',
                                             reportFiles: 'index.html',
                                             reportName: 'Emma Report',
                                    ]
                           }
                  }
        }
}
