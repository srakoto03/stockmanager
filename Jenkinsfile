pipeline {
    agent {
        label 'Linux'
    }
    environment {
      	 DOCKERHUB_MDP=credentials('dockerhub')
	 DOCKERHUB_MDP_USR='sdocker03'
	 DOCKERHUB_MDP_PSW='formation2021'
    }
    stages { 	 
        stage('Test unitaires') {  
             steps { 
                sh 'mvn test' 
             } 
        } 
        stage('Compilation') { 
            steps { 
                sh 'mvn -B -DskipTests clean package' 
            }
        } 
        stage('Publication du Binaire') {
            steps {
                sh "curl -u admin:formation-2021 --upload-file target/stockmanager-0.0.1-SNAPSHOT.jar 'http://10.10.20.31:8081/repository/stockmanager/stockmanager-0.0.1-SNAPSHOT.jar'"  
            }	   
	}
	stage('Téléchargement du binaire') { 
                steps { 
                    sh "wget -P /home/jenkins/tomcat/webapps http://10.10.20.31:8081/repository/stockmanager/stockmanager-0.0.1-SNAPSHOT.jar" 
                } 
        }
	stage ('Test de performance avec SonaQube'){
		steps {
		      withSonarQubeEnv('SonarQube') {		        
			sh 'mvn clean verify sonar:sonar'
		      }
		}	
	 }
	 stage ('Validation de l\'application') {  
                steps { 
                    sh "curl -u admin:formation-2021 --upload-file /home/jenkins/tomcat/webapps/stockmanager-0.0.1-SNAPSHOT.jar 'http://{10.10.20.31}:8081/repository/stockmanager/stockmanager.jar'" 
                } 
         }  
	 stage ('Création de l\'image Docker'){
		 steps { 
			 sh " wget -P /home/jenkins/tomcat/webapps http://10.10.20.31:8081/repository/stockmanager/stockmanager-0.0.1-SNAPSHOT.jar"
			 sh " mv /home/jenkins/tomcat/webapps/stockmanager-0.0.1-SNAPSHOT.jar ./target/ "
			 sh " sudo docker build -t stockmanager:latest . "
		 }
	 }
	 stage ('Stockage de l\'image Docker'){
		 steps { 
			 sh "docker login -u ${env.DOCKERHUB_MDP_USR} -p ${env.DOCKERHUB_MDP_PSW}" 
			 sh "sudo docker push sdocker03/stockmanager:latest "
		 }
	 }
	    
    }  
}
