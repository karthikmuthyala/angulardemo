pipeline{
        agent any
        stages{
        stage ('checkout'){
        steps{
        checkout scm
        }
        }
        stage ('Build'){
        parallel {
        stage("Angular Build") {
        agent {
        docker { image 'node:10' }
        }
        steps {
        sh ' echo Installing packages '
        sh ' npm install '
        sh ' npm install -g @angular/cli@8 '
        sh ' echo Building Angular Project '
        sh ' ng build '
        }
        }
        stage("S3 Build") {
        steps {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 's3toj', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        sh 'aws s3api create-bucket --bucket karthik-muthyala-ld-2-comp --region us-east-1'
        }
        }

        }
        }
        }
        stage ('push'){
        steps{
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 's3toj', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        sh 'aws s3 ls'
        sh 'cd ../sample_angular89@2/dist'
        sh 'echo $PWD'
        //sh 'aws s3 ls s3:// karthik-muthyala-ld-2-comp’
        sh 'aws s3 sync ../sample_angular89@2/dist s3://karthik-muthyala-ld-2-comp/ --region us-east-1'
        }
        }

        }   
        }
        }
        
