node('mailer') {
	def commit_id;
	stage('SCM'){
	checkout scm
	 sh "git rev-parse --short HEAD > .git/commit-id"
	 commit_id = readFile('.git/commit-id').trim()
	}
	stage ('test') {
	  def myTestContainer = docker.image('node:latest')
	    myTestContainer.pull()
            myTestContainer.inside("-u root") {
	    sh 'npm install --only=dev'
            sh 'npm test'
		}
	}
	stage('push'){
	 docker.withRegistry('', 'dockerhubcreds'){
	  def app = docker.build("ianodad/mailer:${commit_id}", ".").push()
		}
	stage('Remove Unused docker image') {
      	 steps{
		 sh "docker rmi ianodad/mailer:${commit_id}"
      		}
	}
}
