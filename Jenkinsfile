pipeline {
    // Run this pipeline on a Jenkins agent/node with label AGENT-1
    agent {
        label 'AGENT-1'
    }

    // Global environment variables available in all stages
    environment {
        COURSE = 'jenkins'
    }

    // Pipeline-level options
    options {
        timeout(time: 30, unit: 'MINUTES')   // Stop build if it runs longer than 30 min be
        disableConcurrentBuilds()            // Prevent multiple runs of this pipeline at the same time
    }

    // Parameters shown when starting the build manually
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }

    // Build pipeline flow
    stages {

        stage('Build') {
            steps {
                script {
                    // Run shell commands on the agent
                    sh """
                        echo "Hello Build"
                        sleep 10
                        env                         // Print all environment variables
                        echo "Hello ${params.PERSON}" // Use build parameter PERSON
                    """
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Testing..'
                }
            }
        }

        stage('Deploy') {
            // Manual approval gate before deploy starts
            input {
                message "Should we continue?"      // Prompt text
                ok "Yes, we should."               // Confirm button text
                submitter "alice,bob"              // Only these users can approve
                parameters {
                    // Extra input parameter requested at approval time
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                script {
                    // Uses PERSON from the input step
                    echo "Hello, ${PERSON}, nice to meet you."
                    echo 'Deploying..'
                }
            }
        }
    }

    // Post actions is the section thar runs after build stages finish (some based on build result)
    post {
        always {
            echo 'I will always say Hello again!' // Runs every time
            deleteDir()                           // Clean workspace
        }
        success {
            echo 'Hello Success'                  // Runs only if build succeeds
        }
        failure {
            echo 'Hello Failure'                  // Runs only if build fails
        }
    }
}