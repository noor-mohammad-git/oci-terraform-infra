pipeline {
  agent any

  environment {
    TF_VAR_tenancy_ocid    = credentials('tenancy-ocid')
    TF_VAR_user_ocid       = credentials('user-ocid')
    TF_VAR_fingerprint     = credentials('fingerprint')
    TF_VAR_private_key     = credentials('private-key')
    TF_VAR_compartment_id  = credentials('compartment-ocid')
    TF_VAR_region          = 'ap-hyderabad-1'  // change to your region
  }

  stages {
    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/your-org/oci-terraform-infra.git', branch: 'main'
      }
    }

    stage('Terraform Init') {
      steps {
        sh 'terraform -chdir=environments/dev init'
      }
    }

    stage('Terraform Plan') {
      steps {
        sh 'terraform -chdir=environments/dev plan -var-file=dev.tfvars -out=tfplan'
      }
    }

    stage('Terraform Apply') {
      steps {
        input message: "Apply Terraform changes?" // Optional approval
        sh 'terraform -chdir=environments/dev apply -auto-approve tfplan'
      }
    }
  }

  post {
    failure {
      mail to: 'you@example.com',
           subject: "Build Failed: ${env.JOB_NAME}",
           body: "See console: ${env.BUILD_URL}"
    }
  }
}
