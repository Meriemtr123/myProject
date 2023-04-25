pipeline {
  environment {
        localhost = "192.168.0.11"
        dockerImage = "myProject"
        tagname = "second"
        registry= "meriemtr/projet"
	    registryCredential = "dockerRegPwd"
    }
    agent any
    stages {
        stage('Clone source code from Git') {
            steps {
                echo "Cloning Project from GitHub; Branch : master"
                git branch: 'master',
                url: 'https://github.com/Meriemtr123/myProject.git'
            }
        }
        stage('MVN CLEAN') {
            steps {
                sh 'mvn -version'
                sh 'mvn clean'
            }
        }
        //stage('MVN TEST (Mockito)') {
          //  steps {
           //     sh 'mvn test'
            //}
        //}
        //stage('MVN SONARQUBE') {
          //  steps {
            //    sh 'mvn sonar:sonar -Dsonar.projectKey=fouad_achat -Dsonar.host.url=http://${localhost}:9000 -Dsonar.login=311d4bf1a2ee143843a922533048832dd3487f39'
            //}
        //}   
        stage('MVN DEPLOY') {
            steps {
                sh 'mvn clean package deploy:deploy-file -DgroupId=tn.esprit -DartifactId=achat -Dversion=1.0 -DgeneratePom=true -Dpackaging=war -DrepositoryId=deploymentRepo -Durl=http://${localhost}:8081/repository/maven-releases/ -Dfile=target/achat-1.0.jar'
            }
        }
        stage('BUILD') { 
            steps { 
                script { 
                    timestamps {
                    dockerImage = docker.build registry
                    }
                }
            } 
        }
        stage('PUSH DOCKERHUB') { 
            steps { 
                script {
                        timestamps {
						  docker.withRegistry ('', registryCredential ) {
							  dockerImage.push()
                        }
                    } 
                }
            } 
            
        }
         stage('RMV IMG') {
            steps {
                sh "docker rmi $registry:latest"
            }
        }
           stage('DOCKER-COMPOSE') {
            steps {
                sh 'docker-compose down --remove-orphans'
                sh 'docker-compose -f docker-compose.yml up -d'
                sh 'docker restart spring-boot-docker-container'
            }
        }
        
    }
}