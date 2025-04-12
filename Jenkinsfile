pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
               sh 'echo $PATH; pip3 install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
               sh 'pytest test_pytest_sbdl.py'
            }
        }
        stage('Package') {
	    when{
		    anyOf{ branch "master" ; branch 'release' }
	    }
            steps {
               sh 'zip -r sbdl.zip lib'
            }
        }
	stage('Release') {
	   when{
	      branch 'release'
	   }
           steps {
              sh "scp -i /home/hugostark/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-qa"
           }
        }
	stage('Deploy') {
	   when{
	      branch 'master'
	   }
           steps {
               sh "scp -i /home/hugostark/cred/edge-node_key.pem -o 'StrictHostKeyChecking no' -r sbdl.zip log4j.properties sbdl_main.py sbdl_submit.sh conf prashant@40.117.123.105:/home/prashant/sbdl-prod"
           }
        }
    }
}
