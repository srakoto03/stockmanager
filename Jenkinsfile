pipeline {
    agent {
        label 'Linux'
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
    }  
}
