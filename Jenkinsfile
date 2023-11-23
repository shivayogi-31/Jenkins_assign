
pipeline {
    agent any

    parameters {
        choice choices: ['Dev', 'QA', 'UAT', 'Prod'], description: 'Select the environment', name: 'branch_name'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.branch_name}", url: "https://github.com/shivayogi-31/Jenkins_assign.git"
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run build stage
                    sh 'mvn clean package'
                }
            }
        }

        stage('Test') {
            when {
                // Run test stage only for QA, UAT, and Prod branches
                expression { return params.branch_name ==~ /(QA|UAT|Prod)/ }
            }
            steps {
                script {
                    // Run test stage
                    sh 'mvn test'
                }
            }
        }

        stage('Deploy') {
            when {
                // Run deploy stage only for UAT and Prod branches
                expression { return params.branch_name ==~ /(UAT|Prod)/ }
            }
            steps {
                script {
                    // Run deploy stage
                    sh 'mvn install'
                }
            }
        }
    }

    post {
        always {
            script {
                sh 'chmod +x script.sh'
                sh './script.sh'
            }
           
            // Clean up workspace
           cleanWs()
        
        }
    }
}

