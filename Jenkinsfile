// Scripted

// Declarative
pipeline {
	agent any
	//agent {docker {image 'maven:3.6.3'}}
	environment {
		dockerHome = tool 'MyDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		stage('Chekout') {
			steps {
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD NUMBER - $env.BUILD_NUMBER"
				echo "BUILD ID - $env.BUILD_ID"
				echo "JOB NAME - $env.JOB_NAME"
				echo "BUILD TAG - $env.BUILD_TAG"
				echo "BUILD URL - $env.BUILD_URL"
			}
		}

		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}

		stage('Dependency Check') {
			steps {
				sh "mvn dependency:tree"
			}
		}

		stage('Test') {
			steps {
				sh "mvn test"
			}
		}

		stage('Integeration Test') {
			steps {
				sh "mvn failsave:integeration-test failsafe:verify"
			}
		}

		stage('Package') {
			steps {
				sh "mvn package -DskipTests"
			}
		}

		stage('Build Docker Image') {
			steps {
				//"docker build -t saif178/currency-exchange-devops:$env.BUILD_TAG"
				script {
					dockerImage = docker.build("saif178/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}

		stage('Push Docker Image') {
			steps {
				script {
					docker.withRegistry{
						dockerImage.push();
						dockerImage.push('latest');
					}
				}
			}
		}
	}

		

	post {
		always {
			echo 'Im awesome. I run always'
		}
		success {
			echo 'I run when you are successful'
		}
		failure {
			echo 'I run when you fail'
		}
	}
}