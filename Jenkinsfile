pipeline{
 agent {
  label 'master'
    }

        environment {
        def prefijo="jenkinsfile_demo_php"
        def ejecucionActual="$ejecucionActual"
        def anterior="((BUILD_NUMBER-1))"
        def scannerHome = tool 'sonar_scaner'
        }
        
        stages{ 

          stage('Prepare properties sonar') {
              steps{
                  sh "sudo mv sonar-scanner.properties /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar_scaner/conf/sonar-scanner.properties"
//                   sh "sudo mv /var/lib/jenkins/workspace/Plantillas_Jenkinsfiles/Almaviva/Back-End_Gradle/sonar-scanner.properties /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar_scaner/conf/sonar-scanner.properties"
                  //sh "sudo mv /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar_scaner/conf/almafront-sonar-scanner.properties /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar_scaner/conf/"
              }
          }
          
          stage('SonarQube analysis') {
              steps {
                  withSonarQubeEnv('sonarqube') {
                  sh "${scannerHome}/bin/sonar-scanner"
                  }
              }
          }
                  
        }
}   
    
