pipeline {  
    triggers {      
        pollSCM('* * * * *')  
    }
    agent {    
        kubernetes {
            // this label will be the prefix of the generated pod's name      
            label 'kubeagent'
            cloud "kubernetes"      
            yaml '''
apiVersion: v1
kind: Pod
metadata:
  labels:
    job: build-pod
  component: ci
spec:
  containers:
    - name: python
      command:
        - cat
      image: python:3.7
      tty: true

    - name: docker
      image: docker
      command:        
        - cat
      tty: true
      volumeMounts:        
        - mountPath: /var/run/docker.sock
          name: docker-sock

  volumes:    
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
'''    
                }  
    }
    stages {    
        stage('Test python') {      
            steps {        
                container('python') {          
                    sh "pip install -r requirements.txt"          
                    sh "python test.py"        
                }      
            }    
        }  
        
        stage('Build image') {      
            steps {        
                container('docker') {          
                    sh "docker build -t localhost:4000/pythontest:latest ."          
                    sh "docker push localhost:4000/pythontest:latest"        
                }      
        }    
    }
    }
    
}