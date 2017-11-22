pipeline {   
         agent any
         triggers {
                  pollSCM('*/5 * * * *')
         }
         stages {
                  stage('Build') {                         
                           steps {
                                    echo 'Building...'
                                    sh 'cd ./sample-java-project && mvn clean compile assembly:single'
                                    archiveArtifacts './sample-java-project/target/sample-java-project-jar-with-dependencies.jar'
                           }
                  }
                  stage('Test') {                         
                           steps {
                                    echo 'Testing & Emmaing...'
                                    sh 'cd sample-java-project && mvn emma:emma'
                                    
                                    // jUnit speichern
                                    junit 'sample-java-project/target/surefire-reports/*.xml'
                                    
                                    // Publish Emma as HTML Report
                                    publishHTML target: [
                                             allowMissing: true,
                                             alwaysLinkToLastBuild: false,
                                             keepAll: true,
                                             reportDir: 'sample-java-project/target/site/emma',
                                             reportFiles: 'index.html',
                                             reportName: 'Emma Report',
                                    ]
                           }
                  }
                  stage('Reports') {                         
                           steps {
                                    echo 'Reporting: Findbugs & Checkstyle...'
                                    sh 'cd sample-java-project && mvn findbugs:findbugs checkstyle:checkstyle -Dcheckstyle.config.location="sample-java-project/checkstyle.xml"'
                                    
                                    // Publish Checkstyle
                                    step([$class: 'hudson.plugins.checkstyle.CheckStylePublisher', pattern: 'sample-java-project/target/checkstyle-result.xml', unstableTotalAll:'80000'])
                                    
                                    // Publish FindBugs
                                    step([$class: 'FindBugsPublisher', pattern: 'sample-java-project/target/findbugsXml.xml', unstableTotalAll:'2000'])
                           }
                  }
                  stage('Deploying') {
                           steps {
                                    echo 'Deploying...'
                                    sh 'cp sample-java-project/target/sample-java-project-jar-with-dependencies.jar /var/lib/jenkins/deploy'
                                    //sh 'cd /var/lib/jenkins/deploy/ && java -jar sample-java-project-jar-with-dependencies.jar'
                           }
                  }
        }
}
