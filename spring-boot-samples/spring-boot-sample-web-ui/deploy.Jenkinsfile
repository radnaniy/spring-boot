pipeline {
    agent {
        label 'agent || t2.medium'
    }
    environment {
        AWS_DEFAULT_REGION = 'eu-central-1'
        ECR_REPOSITORY = '049013287065.dkr.ecr.eu-central-1.amazonaws.com/springboot'
    }
    parameters { 
        choice(name: 'ENVIRONMENT', choices: ['Development', 'QA'], description: 'Choose the environment to deploy')        
        text(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Enter image tag to deploy')
    }
    stages {
        stage ('Deploy container') {
            steps {
                ansiblePlaybook( 
                    playbook: 'spring-boot-samples/spring-boot-sample-web-ui/ansible/playbook.yml',
                    inventory: "spring-boot-samples/spring-boot-sample-web-ui/ansible/${params.ENVIRONMENT}", 
                    credentialsId: '1ca318ad-48bd-49bb-8d5a-5afdae25b64a',
                    disableHostKeyChecking: true,
                    extraVars: [
                        deploy_image_tag: params.IMAGE_TAG,
                        deploy_ecr_url: env.ECR_REPOSITORY,
                        deploy_ecr_region: env.AWS_DEFAULT_REGION
                    ]
                )
            }
        }
    }
}
