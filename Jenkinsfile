pipeline {

  tools{
	jdk '8'
	maven '3.8.5'
}
  environment {
    registry = "ganeshbarma/miniproj"
    registryCredential = 'SPE'
    ImgId = ''
  }
  agent any
  stages {
    stage('GIT CLONE') {
      steps {
        git 'https://github.com/GaneshBarma/MiniProj'
      }
    }
    stage('MVN COMPILE') {
      steps {
        echo "Compiling the source Java classes of the project"
		sh "mvn compile"
      }
    }
    stage('MVN TEST') {
      steps {
        echo "Running the test cases of the project"
        sh "mvn test"
      }
    }
    stage('MVN INSTALL') {
      steps {
        echo "building the project and installs the project files(JAR) to the local repository"
        sh "mvn clean install"
      }
    }
    stage('Building Docker image') {
      steps{
        script {
          echo "Building Docker image"
          ImgId= docker.build registry + ":latest"
        }
      }
    }
    stage('Deploy Docker Image') {
      steps{
        script {
          echo "Deploying Docker Image to " + registry
          docker.withRegistry( '', registryCredential ) {
            ImgId.push()
          }
        }
      }
    }
    stage('Ansible') {
      steps{
        ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible',  playbook: 'calc.yml', sudoUser: null
      }
    }    
  }
}

