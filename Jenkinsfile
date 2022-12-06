pipeline {  
    agent {    
        kubernetes {
            // this label will be the prefix of the generated pod's name      
            label 'jenkins-agent-my-app'      
            yaml """
            apiVersion: v1
            kind: Pod
            metadata:
                labels:
                component: ci
            spec:
                containers:
                    - command:
                        - cat
                    name: python
                    image: python:3.7
                    tty: true
            """    
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