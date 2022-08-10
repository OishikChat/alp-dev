pipeline {
    agent any
    }
    stages {
         stage('Clone to Packer Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/piyushmj12/golden_image_packer'
            }
        }
          stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/piyushmj12/golden_image_packer']]])
                sh "ls -lrt"
                sh "ls scripts"
              
                
            }
        }
         stage("Parameter selection"){
          steps{
           script{
                properties([
                        parameters([
                            choice(
                                choices: ['aws', 'azure','oci'],
                                name: 'Cloud_Provider',
                                 description: 'Please select appropriate cloud provider'
                            ),
                             choice(
                                choices: ['JAVA', 'Python','Docker'],
                                name: 'provisioner_script',
                                description: 'Please select appropriate template'
                            )
                        ])
                     ])
                echo "Cloud Provider: ${params.Cloud_Provider}"
           echo "Selected template: ${params.Template}"
           }
         }
       }
       
        stage('Running Packer Template') {
            steps {
                sh """
                pwd
                cd ${params.Cloud_Provider}/templates
                
                packer build -var "ami_name=${params.image_name}" -var "region=${params.region}" -var "provisioner_scripts=${params.provisioner_script}" .
                pwd
                """
                
            }
        }
   }
