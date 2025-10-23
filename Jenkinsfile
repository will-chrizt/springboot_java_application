
@Library('my-shared-library') _

pipeline{

    agent any
    
    
    parameters{

        choice(name: 'action' , choices: 'create\ndelete', description: 'choose action to perform')
        string(name: 'ImageName', description: "name of the docker build", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "name of the Application", defaultValue: 'willchrist')
    }
        

            
        stages{

            stage('Git Checkout'){
                when{expression { params.action == 'create' }}
                   
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/will-chrizt/springboot_java_application.git"
            )
            }
            }
            stage('unit test maven'){
                when{expression { params.action == 'create' }}
                steps{
                   script{
                       mvnTest()
               }       
            }
            }
            stage('mvn integration test'){
                when{expression { params.action == 'create' }}
                steps{
                    script{
                        mvnIntegrationTest()
                    }
                }
            }

            // stage('static code analysis: sonarqube'){
            //     when{expression { params.action == 'create' }}
            //     steps{
            //         script{
            //             def ZSonarQubecredentialId = 'server2'
            //             statiCodeAnalysis(ZSonarQubecredentialId)

            //         }
            //     }
            // }

            // stage('Quality Gate  Status Check : sonarqube'){
            //     when{expression { params.action == 'create' }}
            //     steps{
            //         script{
            //             def ZSonarQubecredentialId = 'server2'
            //             statiCodeAnalysis(ZSonarQubecredentialId)

            //         }
            //     }
            // }

            stage('Maven Build : maven'){
                when{expression { params.action == 'create' }}
                steps{
                    script{
                       mvnBuild()

                    }
                }
            }

            stage('Docker Image Build'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerBuild("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }

        stage('Docker Image Scan: trivy '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImageScan("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }

        stage('Docker Image Push : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImagePush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }   
        stage('Docker Image Cleanup : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImageCleanup("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }      
    }

            
        }


    