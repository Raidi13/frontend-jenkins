pipeline {
    agent {
        label 'Agent-1'
    }
    options{
        timeout(time: 10,unit: 'MINUTES')
        disableConcurrentBuilds ()
        // retry (1)
    }
     environment {
        DEBUG = 'true'
        appVersion= "" // you can use the image across the pipeline
        region = "us-east-1"
        aws_account =  "983015583980"
        project= "expense"
        environment = "dev"
        component = 'frontend'
    }
    stages {
        stage('Read the version') {
            steps {
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "App version: ${appVersion}"
                }
            }
        }
      
           stage('Docker build') {
            
            steps {
                // withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                //     sh """
                //     docker build -t raidi/backend:${appVersion} .
                //     docker images
                //     """[]
                // ecr-image $region | docker login --username AWS --password-stdin $
                // (aws_account_id).dkr.ecr.us-east-1.amazonaws.com
                // }
                //  use  the ecr image, build,push
            }
        }
        stage('Deploy') {
            steps {
                withAWS(region: 'us-east-1', credentials: "aws-credentials")
                    sh """
                        aws eks update -kubecofig --region ${ region }--name ${ project} --${environment}
                        cd helm
                        sed -i 's/IMAGE_VERSION/${appVersion}/g' values-${environment}.yaml
                        helm upgrade --install ${component} -n ${project} -f values -${environment}.yaml .
                    """
            }

            }
        }
        stage('print params'){
            steps{
                echo "Hello ${params.PERSON}"
                echo "Biography: ${params.BIOGRAPHY}"
                echo "Toggle: ${params.TOGGLE}"
                echo "Choice: ${params.CHOICE}"
                echo "Password: ${params.PASSWORD}"

            }
        }
    //       stage('Approval'){
    //          input {
    //              message "Should we continue?"
    //             ok "Yes, we should."
    //             submitter "alice,bob"
    //              parameters {
    //                 string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    //              }
    //          }
    //         steps {
    //             echo "Hello, ${PERSON}, nice to meet you."
    //         }
    //  }
    }

post {
    always{
        echo 'This sections runs always'
        deleteDir() 
    }
    success{
        echo 'This section runs when pipeline success'
    }
    failure{
        echo 'This section runs when pipline failure'
    }
    }
}


