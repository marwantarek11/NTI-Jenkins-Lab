# Jenkins Project Overview

## Shared Libirary Repository
https://github.com/marwantarek11/shared_library.git

## Pipeline Overview

The Jenkins pipeline follows these stages to build, push, edite and deploy Docker images to dockerhub and an Minikube cluster:

1. **Build Docker Image:** Build docker image.

2. **Push Docker Image To DockerHub:** Push docker image to docker hub.

3. **Edit new image in deployment.yaml file:** Edit new image in deployment.yaml file.

4. **Deploy to k8s:**  Deploy image to k8s minikube cluster.


## Pipeline Steps

### Build Docker Image:

```
 stage('Build Docker image from Dockerfile in GitHub') {
            steps {
                script {
                 	
                 		buildDockerImage("${imageName}")
                      
                }
            }
        }
```



### Push Docker Image To DockerHub:

```
stage('Push image to Docker hub') {
            steps {
                script {
                 	
                 		pushDockerImage("${dockerHubCredentialsID}", "${imageName}")
                      
                }
            }
        }

```

### Edit new image in deployment.yaml file:

```
stage('Edit new image in deployment.yaml file') {
            steps {
                script { 
                	dir('k8s') {
				        editNewImage("${imageName}")
                    	}
                }
            }
        }
```
### Deploy to k8s:

```
stage('Deploy on k8s Cluster') {
            steps {
                script { 
                	dir('k8s') {
				         deployOnKubernetes("${k8sCredentialsID}", "${branchName}")
                    }
                }
            }
        }

```


### Post-Build Actions
In case of pipeline success or failure, the following messages will be displayed:
```
 post {
        always {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline always succeeded"
        }
        success {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline succeeded"
        }
        failure {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline failed"
        }
    }
```
----
### Successfully Run Pipeline
![](https://github.com/marwantarek11/Nti-Jenkins-Lab/blob/main/screenshots/jenkins%20.png)
![](https://github.com/marwantarek11/Nti-Jenkins-Lab/blob/main/screenshots/jenkins%20project.png)

### Successfully Run Development Pipeline On k8s minikube cluster
![](https://github.com/marwantarek11/Nti-Jenkins-Lab/blob/main/screenshots/minikube%20result.png)

### Successfully Run Production Pipeline On k8s minikube cluster
![](https://github.com/marwantarek11/Nti-Jenkins-Lab/blob/main/screenshots/running.png)
