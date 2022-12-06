pipeline {  
    triggers {      
        pollSCM('* * * * *')  
    }
    agent {    
        kubernetes {
            // this label will be the prefix of the generated pod's name      
            label 'jenkins-agent-my-app'
            cloud "kubernetes"      
            yaml '''
apiVersion: v1
kind: Pod
metadata:
  namespace: default
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
    }
}