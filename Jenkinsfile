pipeline {
    agent {
        kubernetes {
            // Rather than inline YAML, in a multibranch Pipeline you could use: yamlFile 'jenkins-pod.yaml'
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven
    command:
    - sleep
    args:
    - infinity
'''
            // Also can create a Kubernetes Pod Template and reference it instead of YAML

            // Can also wrap individual steps:
            // container('shell') {
            //     sh 'hostname'
            // }
            defaultContainer 'maven'
        }
    }
    

    triggers {
        pollSCM('H/1 * * * *') // Poll for changes every minute
    }

    stages {

                stage('Checkout SCM') {
            steps {
                // Checkout branches matching the pattern **/test/*
                script {
                    def branchSpec = '**/test/*'
                    checkout([$class: 'GitSCM',
                              branches: [[name: branchSpec]],
                              userRemoteConfigs: [[url: 'https://github.com/Gangwal-Mayank/onlinebanking.git']]
                    ])
                }
            }
        }
        
        
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
