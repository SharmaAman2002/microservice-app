pipeline{
    agent any 
    stages{
        stage("Git checkout"){
            steps{
                git branch: 'main', url: 'https://github.com/vjdbj/microservice.git'
            }
        }
        stage("ECR login"){
            def ECR_URL = "863804372452.dkr.ecr.us-east-1.amazonaws.com"
            script{
                sh """
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_URL}
            """
            }
        }
        stage("image build : cart_micro"){
            dir('cart-microservice-nodejs'){
                script{
                sh """
                    docker build -t cart .
                    docker tag cart:latest 863804372452.dkr.ecr.us-east-1.amazonaws.com/cart:latest
                    docker push 863804372452.dkr.ecr.us-east-1.amazonaws.com/cart:latest
                """
                }
            }
        }
        stage("deploy : cart_micro"){
            script{
                sh "kubectl rollout restart deployment.apps/cart:microservices"
            }
        }
        stage("image build : shoes_micro"){
            dir('shoes-microservice-spring-boot'){
                script{
                sh """
                    docker build -t shoes .
                    docker tag shoes:latest 863804372452.dkr.ecr.us-east-1.amazonaws.com/shoes:latest
                    docker push 863804372452.dkr.ecr.us-east-1.amazonaws.com/shoes:latest
                """
                }
            }
        }
        stage("deploy : shoes_micro"){
            script{
                sh "kubectl rollout restart deployment.apps/shoes:microservices"
            }
        }
        stage("image build : ui_micro"){
            dir('shoes-microservice-spring-boot'){
                script{
                sh """
                    docker build -t ui .
                    docker tag ui:latest 863804372452.dkr.ecr.us-east-1.amazonaws.ui:latest
                    docker push 863804372452.dkr.ecr.us-east-1.amazonaws.com/ui:latest
                """
                }
            }
        }
        stage("deploy : ui_micro"){
            script{
                sh "kubectl rollout restart deployment.apps/ ui-web-app:microservices"
            }
        }
        stage("image build : api_micro"){
            dir('shoes-microservice-spring-boot'){
                script{
                sh """
                    docker build -t zuul-api .
                    docker tag zuul-api:latest 863804372452.dkr.ecr.us-east-1.amazonaws.zuul-api:latest
                    docker push 863804372452.dkr.ecr.us-east-1.amazonaws.com/zuul-api:latest
                """
                }
            }
        }
        stage("deploy : api_micro"){
            script{
                sh "kubectl rollout restart deployment.apps/ zuul-api:microservices"
            }
        }
    }
}