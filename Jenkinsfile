pipeline {
     agent any
     stages {
         stage('Build') {
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
         stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
         stage('Security Scan') {
              steps { 
                 aquaMicroscanner imageName: 'alpine:latest', notCompliesCmd: 'exit 1', onDisallowed: 'fail',outputFormat: 'html'
                 //notCompleted: 'exit 1', 
              }
         }         
        // stage('Upload to AWS') {
        //      steps {
        //          withAWS(region:'us-east-2',credentials:'aws-static') {
        //          sh 'echo "Uploading content with AWS creds"'
        //              s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'static-jenkins-pipeline')
        //          }
        //      }
        //}
          
        stage('Upload to AWS 2') {
            steps {
                // Example AWS credentials
                withCredentials(
                [[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    credentialsId: 'MyAWSCredential',  // ID of credentials in Jenkins
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    echo "Listing contents of an S3 bucket."
                    sh "AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
                        AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
                        AWS_REGION=us-east-2 \
                        //aws s3 ls clouductivity-demo"
                        sh("aws s3 cp index.html s3://static-jenkins-pipeline/index.html")
                }
            }
        }  
          
          
     }
}
