pipeline{

    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        COURSE = "Jenkins"
        appVersion = ""
        ACC_ID = "611669940532"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }
    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    // BUILD SECTION
    stages{
        stage('Read Version'){
            steps{
                script{
                    def packageJSON = readJSON file: 'package.json'
                    appVersion = packegeJSON.version
                    echo "app version is: ${appVersion}"
                }
            }

        }
        stage('Install Dependencies'){
            steps{
                script{
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Build Image'){
            steps{
                script{
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                            docker build -t ${PROJECT}/${COMPONENT} .

                            docker tag ${PROJECT}/${COMPONENT}:${appVersion} ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}

                            docker images

                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
                    }
                
                }
            }
        }
    }
        post{
            always{
                echo 'I will always say Hello again!'
                cleanWS()
            }
            success{
                echo 'I will run if success'
            }
            failure{
                echo 'I will run if failing'
            }
            aborted{
                echo 'Pipeline is aborted'
            }
        }
    }
