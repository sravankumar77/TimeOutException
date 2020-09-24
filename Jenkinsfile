pipeline {
    agent any

    stages {
    
        stage("Git checkout"){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, 
                extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/sravankumar77/TimeOutException.git']]])
            }
        }
        stage("maven build"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Develop") {
            options {
                timeout(time: 3, unit: "SECONDS")
            }

            steps {
                           
                script {
                    Exception caughtException = null

                    catchError(buildResult: 'SUCCESS', stageResult: 'ABORTED') { 
                        try { 
                            echo "Started stage A"
                            sleep(time: 5, unit: "SECONDS")
                        } catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
                            error "Caught ${e.toString()}" 
                        } catch (Throwable e) {
                            caughtException = e
                        }
                    }

                    if (caughtException) {
                        error caughtException.message
                    }
                }
            }
        }

        stage("Production") {
            steps {
                echo "this is production"
            }
        }
    }
}
