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
        }
}
