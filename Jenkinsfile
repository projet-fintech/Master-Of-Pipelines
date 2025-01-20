pipeline {
    agent any
    stages {
         stage('Checkout Triggering Repo') {
            steps {
                script {
                     checkout scm // Cloner uniquement le repo qui a trigger le pipeline
                }
            }
        }
        stage('Analyse Changes & Orchestrate Deployments') {
             steps {
                script {
                     def changedFiles = sh(returnStdout: true, script: 'git diff --name-only HEAD~1 HEAD').trim().split('\n')
                     if (changedFiles.any { it.startsWith("repo/com/banque/events-lib/") } ) {
                          println "events-lib files changed, triggering redeployment"
                         build job: 'events-lib/main', wait: true
                       }
                      if (changedFiles.any { it.startsWith("User-microservice") } ) {
                          println "User-microservice files changed, triggering redeployment"
                          build job: 'User-Micorservice/main', wait: true, parameters: [
                            string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                         ]
                      }
                       if (changedFiles.any { it.startsWith("Authentification-Service") } ) {
                            println "Authentification-Service files changed, triggering redeployment"
                           build job: 'Authentication-Microservice/main', wait: true, parameters: [
                                string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                            ]
                      }
                      if (changedFiles.any { it.startsWith("Account-Microservice") } ) {
                          println "Account-Microservice files changed, triggering redeployment"
                           build job: 'Account-Microservice/main', wait: true, parameters: [
                                string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                            ]
                      }
                       if (changedFiles.any { it.startsWith("inssuranceManagement") } ) {
                           println "inssuranceManagement files changed, triggering redeployment"
                           build job: 'inssuranceManagement/main', wait: true, parameters: [
                                 string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                             ]
                      }
                       if (changedFiles.any { it.startsWith("Loan-Management") } ) {
                           println "Loan-Management files changed, triggering redeployment"
                           build job: 'Loan-Management/main', wait: true, parameters: [
                                string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                            ]
                       }
                      if (changedFiles.any { it.startsWith("Notifications-Microservice") } ) {
                          println "Notifications-Microservice files changed, triggering redeployment"
                           build job: 'Notifications-Microservice/main', wait: true, parameters: [
                                string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                            ]
                      }
                       if (changedFiles.any { it.startsWith("Transaction-Microservice") } ) {
                            println "Transaction-Microservice files changed, triggering redeployment"
                           build job: 'Transaction-Microservice/main', wait: true, parameters: [
                                string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                            ]
                      }
                       if (changedFiles.any { it.startsWith("Car-Inssurance") } ) {
                         println "Car-Inssurance files changed, triggering redeployment"
                           build job: 'Car-Inssurance/main', wait: true, parameters: [
                                string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                           ]
                     }
                    if (changedFiles.any { it.startsWith("Loan-Approval") } ) {
                         println "Loan-Approval files changed, triggering redeployment"
                          build job: 'Loan-Approval/main', wait: true, parameters: [
                                string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                            ]
                     }
                      if (changedFiles.any { it.startsWith("Medical-Inssurance") } ) {
                          println "Medical-Inssurance files changed, triggering redeployment"
                           build job: 'Medical-Inssurance/main', wait: true, parameters: [
                                string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                            ]
                      }
                        else {
                            println "No relevant changes found. Skipping deployments."
                       }
                }
            }
        }
    }
}
