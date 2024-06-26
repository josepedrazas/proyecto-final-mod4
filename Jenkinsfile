pipeline{
    agent any

    environment {
        IMAGE_NAME = "jpedraz/proyecto-final-mod4"
        DOCKERHUB_CREDS = credentials("jenkins-proyecto-final")
        DOCKERHUB_CREDS_USR = credentials("docker-user")
        NameContainer = "proyecto-final-mod4"
    }

    stages{
        stage('Build image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:v${BUILD_NUMBER} .'
            }
        }
        
        stage('Run Unit Test') {
            steps {
                sh 'pip3 install -r requirements.txt && python3 -m unittest test_app.py'
            }
        }
        
        stage("Docker push"){
            steps{
                //sh 'echo ${DOCKERHUB_CREDS_PSW}'
                sh 'echo ${DOCKERHUB_CREDS} | docker login -u ${DOCKERHUB_CREDS_USR} --password-stdin' 
                sh 'docker push ${IMAGE_NAME}:v${BUILD_NUMBER}'
            }
        }

        /*
        stage("Docker run"){
            steps{
                input message: 'Continue?'
                sh """
                if docker ps -a | grep -q ${env.NameContainer}; then 
                    docker stop ${env.NameContainer} && docker rm ${env.NameContainer}
                fi
                docker run -d --name ${env.NameContainer} -p 5000:5000 ${IMAGE_NAME}:${BUILD_NUMBER}
                """
            }
        }
        */
        
        // stage("Update Deployment Manifest") {
        //     steps {
        //         script {
        //             // Construir el nombre de la imagen con el número de build
        //             def newImage = "${IMAGE_NAME}:${BUILD_NUMBER}"
        //             // Usar sed para actualizar el archivo de manifiesto
        //             sh "sed -i 's|IMAGE_PLACEHOLDER|${newImage}|' proyecto-final-mod4/k8s/manifest.yaml"
        //             sh "cat proyecto-final-mod4/k8s/manifest.yaml "
        //         }
        //     }
        // }

        // stage("Deploy in Kubernetes") {
        //      steps {
        //          //input message: 'Continue?'
        //          script {
        //              withCredentials([file(credentialsId: 'minikube_config', variable: 'KUBECONFIG')]) {
        //                  sh 'kubectl apply -f proyecto-final-mod4/k8s/manifest.yaml'
        //              }
        //          }
        //      }
        // }
    
    }
    
    post {
        always {
            sh 'docker logout'
        }
    } 
}