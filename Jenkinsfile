pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-21-openjdk'  // Adjust to your installed version
        PATH = "${JAVA_HOME}/bin:${env.PATH}:/var/lib/jenkins/.local/bin"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/dragonamit10/testingjenkins.git'
            }
        }

        stage('Build Java Project') {
            steps {
                echo 'Building Java project...'
		echo "Current Branch: ${env.GIT_BRANCH} ${env.BRANCH_NAME}"
                dir('java_project') { 
		    echo "${PATH}"
                    sh 'mvn clean install'  
                }
            }
        }

        stage('Run Python Tests') {
            steps {
                echo 'Running Python tests...'
                dir('python_project') { 
                    sh 'pip install -r requirements.txt'
                    sh 'pytest tests/'
                }
            }
        }

        stage('Deploy') {
            when {
	        expression {
		    echo "Checking branch: ${env.GIT_BRANCH}"
		    return env.GIT_BRANCH == 'origin/main' 
                }
            }
            steps {
                echo 'Deploying application...'
		sh 'chmod 754 deploy.sh'
                sh './deploy.sh' 
            }
        }
    }

    post {
        success {
            echo "✅ Build and tests passed. Pipeline completed successfully."
        }
        failure {
            echo "❌ Build or tests failed. Check the logs for errors."
        }
    }
}
        
