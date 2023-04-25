pipeline {
    agent any
    
    stages {
        stage('Git') {
            steps {
                echo "Getting Project from Git";
                sh "rm -rf myProject"
                sh "git clone https://github.com/Meriemtr123/myProject.git"
                  }
            }
            
        stage('MVN Clean'){  
            steps {
                sh "mvn clean"
                  }
        }        
        
        stage('MVN Install'){
            steps {
                sh "mvn install"
                  }
        }       
        
        stage('MVN Test'){
            steps {
                sh "mvn test"
                  }
        }          
        
        stage('MVN Package'){
            steps {
                sh "mvn package"
                  }
        }          
        
        stage('MVN SONARQUBE'){
            steps{
                sh 'mvn sonar:sonar \
  -Dsonar.projectKey=myproject \
  -Dsonar.host.url=http://192.168.1.162:9000 \
  -Dsonar.login=0a7920153f8d430b6e508640db3dad567bdd05a5'
                 }
        }    
        stage('MVN NEXUS'){
            steps {
                sh 'mvn clean package deploy:deploy-file -DgroupId=tn.esprit -DartifactId=ExamThourayaS2 -Dversion=1.0 -DgeneratePom=true -Dpackaging=war -DrepositoryId=deploymentRepo -Durl=http://${localhost}:8081/repository/maven-releases/ -Dfile=target/ExamThourayaS2-1.0.jar'
                  }
        }          
   }
}
