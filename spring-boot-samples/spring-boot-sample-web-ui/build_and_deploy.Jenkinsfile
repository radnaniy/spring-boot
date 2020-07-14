pipeline {
    agent {
        label 't2.medium'
    }  
    environment {
        AWS_DEFAULT_REGION = 'eu-central-1'
        ECR_REPOSITORY = '049013287065.dkr.ecr.eu-central-1.amazonaws.com/springboot'
        GIT_COMMIT_SHORT = env.GIT_COMMIT.substring(0, 8)
    }
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['Development', 'QA'], description: 'Choose the environment to deploy')
    }
    stages {
        stage ('Build artefact') {
            steps {
                sh './mvnw -f spring-boot-samples/spring-boot-sample-web-ui/pom.xml clean package -DskipTests'
            }
        }
        stage ('Docker build&push') {
            steps {
                sh "aws ecr get-login-password | docker login --username AWS --password-stdin ${ECR_REPOSITORY}"
                sh "docker build -f spring-boot-samples/spring-boot-sample-web-ui/Dockerfile -t ${ECR_REPOSITORY}:${GIT_COMMIT_SHORT} -t ${ECR_REPOSITORY}:latest ."
                sh "docker push ${ECR_REPOSITORY}:${GIT_COMMIT_SHORT}"
                sh "docker push ${ECR_REPOSITORY}:latest"
            }
        }
        stage ('Deploy container') {
            steps {
                ansiblePlaybook( 
                    playbook: 'spring-boot-samples/spring-boot-sample-web-ui/ansible/playbook.yml',
                    inventory: "spring-boot-samples/spring-boot-sample-web-ui/ansible/${params.ENVIRONMENT}", 
                    credentialsId: '1ca318ad-48bd-49bb-8d5a-5afdae25b64a',
                    disableHostKeyChecking: true,
                    extraVars: [
                        deploy_image_tag: env.GIT_COMMIT_SHORT,
                        deploy_ecr_url: env.ECR_REPOSITORY,
                        deploy_ecr_region: env.AWS_DEFAULT_REGION
                    ]
                )
            }
        }
    }
}
