pipeline {
    agent any
    tools {
        maven 'maven'
    }

    stages {
        stage('Git Clone') {
            steps {
                script {
                    checkout([$class: 'GitSCM',
                        branches: [[name: 'main']],
                        userRemoteConfigs: [[url: 'https://github.com/chK1717/first_test_jenkis.git']]
                    ])
                }
            }
        }

        stage('Build') {
            steps {
                dir('POV-JAVA') {
                    bat 'mvn clean install'
                }
            }
        }

        stage('Create Docker Image') {
            steps {
                dir('POV-JAVA') {
                    bat 'docker build -t chk1717/pos .'
                }
            }
        }

        stage('Cleanup Old Container') {
            steps {
                script {
                    bat 'docker rm -f test-pos || exit 0'
                }
            }
        }

        stage('Run') {
            steps {
                script {
                    bat "docker run --name test-pos -d -p 8585:8282 chk1717/pos"
                }
            }
        }
    }
}
