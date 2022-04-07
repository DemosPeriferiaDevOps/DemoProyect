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
            stage('environments') {
                steps{

                    sh 'sudo -s'
                    sh 'export PATH=/sbin:/bin:/usr/sbin:/usr/bin'
                    sh 'anterior=$((BUILD_NUMBER-1))'
                    sh 'ejecucionActual=$(docker ps |grep "${prefijo}"ContainerBack|cut -f1 -d/ )'
                    sh 'echo actualmente con la ejecucion $BUILD_NUMBER, ejecucion anterior $anterior'
                    sh 'echo Ejecucion actual $ejecucionActual'
                    sh 'echo ${prefijo}'
                }
            }

            stage('Build image') {
                steps{
                    //Construccion de la imagen a apartir de los cambios generados
                    sh """docker build -t "${prefijo}":$BUILD_NUMBER ."""
                }
            }
            
            /*stage('Stop Container') {
                steps{
                    //Detiene la ejecucion anterior
                    sh """
                    if [ ! -z "$prefijo:$anterior" ] 
                    then
                    docker stop $prefijo:$anterior
                    docker rm $prefijo:$anterior
                    fi
                    """
                }
            }*/
            
            
            stage('Start Container') {
                steps{
                    //Iniciar el contenedor
                    sh """docker run -i -p 8080:80 --name "$prefijo"Container$BUILD_NUMBER -d "$prefijo":$BUILD_NUMBER"""
                }
            }

            stage('Prepare properties sonar') {
                steps{
                    sh "sudo mv sonar-scanner.properties /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar_scaner/conf/sonar-scanner.properties"
                    // sh "sudo mv /var/lib/jenkins/workspace/Plantillas_Jenkinsfiles/Almaviva/Back-End_Gradle/sonar-scanner.properties /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar_scaner/conf/sonar-scanner.properties"
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

            stage('Delete properties sonar') {
                steps{
                    sh "sudo rm -r /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar_scaner/conf/sonar-scanner.properties"
                }
            } 
                  
        }
}                    
                    
                    
            //Verificar si se elimina la imagen anterior del repo de imagenes de docker            

 /* 
 El archivo sonar-scanner.properties se encuentra en la raiz del repositorio. alli la configuración de los parametros para que funcione
el analysis de sonar_scaner. Este se copea a la ruta:  /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar_scaner/conf/ ya que alli es 
donde se toma el archivo de configuración de forma global. al final de la ejecucion se elimina este archivo para dar paso a otro analysis.
*/  
    
