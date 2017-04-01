pipeline {
    agent none
    options {
	    buildDiscarder(logRotator(numToKeepStr: '2',artifactNumToKeepStr:'1'))
    }
    stages {
	 stage('Unit Test') {
	        agent {
                label 'master'
            }
            steps {
                sh 'ant -f test.xml -v'
		junit 'reports/result.xml'
            }
        }    
        stage('build') {
            agent {
                label 'master'
            }
            steps {
                sh 'ant -f build.xml -v'
            }
            post {
        success {
            archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
    }
        }
	stage('deploy') {
	        agent {
                label 'master'
            }
            steps {
		    sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
            }
        }
    stage('Running on CentOS') {
	        agent {
                label 'Centos'
            }
            steps {
            sh "wget http://54.145.133.138/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
		    sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
            }
        }    
    }
     
}
