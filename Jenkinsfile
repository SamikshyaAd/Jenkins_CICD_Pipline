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
                 aquaMicroscanner imageName: 'alpine:latest', notCompliesCmd: 'exit 1', onDisallowed: 'fail' , outputFormat: 'html'
              }
         }         
         stage('Upload to AWS') {
              steps {
                  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'blueoceans', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
                  sh """
                        mkdir -p ~/.aws
                        echo "[default]" >~/.aws/credentials
                        echo "[default]" >~/.boto
                        echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" >>~/.boto
                        echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" >>~/.boto
                        echo "aws_access_key_id = ${AWS_ACCESS_KEY_ID}" >>~/.aws/credentials
                        echo "aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}" >>~/.aws/credentials
                  """
                  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'jenkins-cicd-pipeline')
                  }  
              }
         }
     }
}
