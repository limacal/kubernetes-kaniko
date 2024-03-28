pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Run Tests') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }
            steps {
                script {
                    // Example command to run CodeCoverage
                    sh 'mvn clean test -P CodeCoverage'
                    //Running CodeCoverage Test Here 
                    sh 'echo "Running CodeCoverage test"'
                }
            }
        }
        
        stage('Build Container') {
            when {
                //if Branch different than "Playground" Branch
                expression { env.BRANCH_NAME != 'playground' }
            }
            steps {
                script {
                    def imageName
                    def imageVersion

                    // Determine image name and version based on branch
                    if (env.BRANCH_NAME == 'main') {
                        imageName = 'calculator'
                        imageVersion = '1.0'
                    } else {
                        imageName = 'calculator-feature'
                        imageVersion = '0.1'
                    }
                    //Building the image
                    docker.build("repository/${imageName}:${imageVersion}")
                }
            }
        }
        
        stage('Push Container') {
            when {
                allOf {
                    expression { env.BRANCH_NAME != 'playground' }
                    expression { currentBuild.result == 'SUCCESS' }
                }
            }
            steps {
                script {
                    // Build and push container if tests succeed
                    docker.withRegistry('https://hub.docker.com', 'docker-credentials') {
                        def imageName
                        def imageVersion

                        //if Branch equal Main then create Calculator else create Calculator-feature
                        if (env.BRANCH_NAME == 'main') {
                            imageName = 'calculator'
                            imageVersion = '1.0'
                        } else {
                            imageName = 'calculator-feature'
                            imageVersion = '0.1'
                        }

                        //Pushing the docker image
                        docker.image("repository/${imageName}:${imageVersion}").push()
                    }
                }
            }
        }
    }
}
