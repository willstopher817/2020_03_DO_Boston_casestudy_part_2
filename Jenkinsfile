pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = "willstopher/case-study-1"
        CONTAINER_NAME = "case-study-2-container"
        STUB_VALUE = "200"
    }
   
    stages {
        stage('Clone') {
            steps {
                script {
                    sh 'rm -rf 2020_03_DO_Boston_casestudy_part_2'
                    sh 'git clone https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2.git'
                }
            }
        }  
        stage('Setup') {
            steps {
                script {
		    dir('2020_03_DO_Boston_casestudy_part_2') {
                        ansiblePlaybook(playbook: 'ansible-playbook-setup.yaml')
                    }
                }
            }
        }

    }
}
