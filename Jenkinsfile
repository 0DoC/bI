pipeline {
    agent any
    environment{
        APP_PORT='9090'
        APP_FOL="${env.JOB_NAME}"
    }
    tools{    
        maven "maven3"
        jdk "java"
    }
    // Save the job name in a global variable
    stages {
        stage('BUILD'){
            steps {
                sh 'mvn -B package -DskipTests'
            }
        }
        stage('Run App && Integration Test') {
            parallel{
                stage('Running Application') {
                    agent any
                    options {
                        // Timeout counter starts AFTER agent is allocated
                        timeout(time: 60, unit: 'SECONDS')
                    }
                    steps {
                        script{
                            try{
                                dir("${env.WORKSPACE} /../$APP_FOL"){
                                    sh "pwd"
                                    sh "java -jar target/contact.war"
                                }
                            }
                            catch (Throwable e) {
                                echo "Caught ${e.toString()}"
                                currentBuild.result = "SUCCESS" 
                            }
                            // Open the try block
                                // Use the dir("TODO") { Commands } construct to return to the target folder
                                // Run the "contact.war" application from the "target" folder
                            // Open the catch block
                                // Return "success" if the task is stopped after 60 seconds
                            // End of try-catch block
                        // End of the script block
                        }
                    }
                }
                stage('Running Test') {
                    steps {
                        sleep 30
                        echo "Test"
                        sh 'mvn -Dtest=RestIT test'
                        echo 'Test done'
                        // Wait 30 seconds for "contact.war" application to run
                        // Run only the "RestIT" integration test in the "test" phase of maven
                    }
                }
            }
        }        
    }
}
