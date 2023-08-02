//for Docker Swarm
// pipeline {
//   agent any
//   environment {
//     JD_IMAGE = 'flamekaiserr/phase_5_4'
//     registryCredential = '970d8df2-fffb-4375-9c21-191eda3f2bc9'
//   }
//   stages {
//     stage('Clone Repository') {
//       steps {
//         git 'https://github.com/Rohit-Prabhakar-Rao/Phase-5.4_Practice.git'
//       }
//     }

//     stage('Build Docker Image') {
//       steps {
//         script {
//           sh "sudo docker build -t ${JD_IMAGE} ."
//         }
//       }
//     }

//     stage('Push Image to Docker Registry') {
//       steps {
//         script {
//           docker.withRegistry('', registryCredential) {
//             sh "docker push ${JD_IMAGE}"
//           }
//         }
//       }
//     }

//     stage('Create Docker Service') {
//       steps {
//         sh "sudo docker service create --name phase_5 --replicas 1 ${JD_IMAGE}"
//       }
//     }

//     // stage('Configure Container Networking') {
//     //   steps {
//     //     sh 'sudo docker network create --driver overlay mynetwork'
//     //     sh 'sudo docker service update --network-add mynetwork phase_5'
//     //   }
//     // }
    
//     stage('Run Tests') {
//       steps {
//         sh 'sudo docker service logs phase_5'
//       }
//     }

//     stage('Cleanup') {
//       steps {
//         sh 'sudo docker service rm phase_5'
//         // sh 'sudo docker network rm mynetwork'
//       }
//     }
//   }
// }

//for kubernetes
pipeline {
    agent any
    environment {
        JD_IMAGE = 'docker.io/flamekaiserr/phase_5_4'
        K8S_NAMESPACE = 'default' // Replace 'default' with your Kubernetes namespace
        registryCredential = '970d8df2-fffb-4375-9c21-191eda3f2bc9'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Rohit-Prabhakar-Rao/Phase-5.4_Practice.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "sudo docker build -t ${JD_IMAGE} ."
                }
            }
        }

        stage('Push Image to Docker Registry') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                    sh "docker push ${JD_IMAGE}"
                    }
                }
            }
        }

        stage('Configure Kubernetes') {
            steps {
                // sh 'minikube profile minikube'
                sh 'minikube start -p minikube'
                sh 'minikube status'
                sh 'kubectl config use-context minikube'
            }
        }
      //       stage('Configure Kubernetes') {
      //     steps {
      //         sh 'minikube start -p minikube'
      //         sh 'minikube status'
      //         sh 'kubectl config use-context minikube'
      //     }
      // }


        stage('Deploy to Minikube') {
            steps {
                sh "kubectl create deployment my-app --image=${JD_IMAGE} --namespace=${K8S_NAMESPACE}"
                sh "kubectl expose deployment my-app --port=80 --type=LoadBalancer --namespace=${K8S_NAMESPACE}"
            }
        }

        stage('Run Tests') {
            steps {
                sh 'kubectl get pods --namespace=${K8S_NAMESPACE}'
                // You can run additional tests or validation here
            }
        }

        stage('Cleanup') {
            steps {
                sh "kubectl delete service,deployment my-app --namespace=${K8S_NAMESPACE}"
            }
        }
    }
}
