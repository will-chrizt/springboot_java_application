
@Library('my-shared-library') _

pipeline{

    agent any
    environment {
    MAVEN_OPTS = '--add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=jdk.compiler/com.sun.tools.javac.processing=ALL-UNNAMED'
}

    
    parameters{

        choice(name: 'action' , choices: 'create\ndelete', description: 'choose action to perform')
    }
        

            
        stages{

            stage('Git Checkout'){
                when{expression { params.action == 'create' }}
                   
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/vikash-kumar01/mrdevops_java_app.git"
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

            stage('static code analysis: sonarqube'){
                when{expression { params.action == 'create' }}
                steps{
                    script{
                        def ZSonarQubecredentialId = 'sonarqube-server'
                        statiCodeAnalysis(ZSonarQubecredentialId)

                    }
                }
            }    
}
}