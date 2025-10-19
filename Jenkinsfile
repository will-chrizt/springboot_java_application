
@Library('my-shared-library') _

pipeline{

    agent any
    
    parameters{

        choice(name: 'action' , choices: 'create\ndelete', description: 'choose action to perform')
    }
        when{expression { params.action == 'create' }}

            
        stages{

            stage('Git Checkout'){
                   
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/vikash-kumar01/mrdevops_java_app.git"
            )
            }
            }
            stage('unit test maven'){
                steps{
                   script{
                       mvnTest()
               }       
            }
            }
            stage('mvn integration test'){
                steps{
                    script{
                        mvnIntegrationTest()
                    }
                }
            }    
}
}