node{
    def MAVEN_HOME = tool "sheetalMV"
 env.PATH = "${env.PATH}:${MAVEN_HOME}/bin"
	
    stage ('checkout'){
        checkout([$class: 'GitSCM', branches: [[name: '*/development']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sarishbosekar277/credit-service.git']]])
        } 
    stage ('Compile'){
        sh 'mvn clean compile'
    }
stage('unit testing'){
    sh 'mvn test'
}


    stage('Code quality analysis') 
	{
		withSonarQubeEnv('mysonar') 
		{
                 sh 'mvn sonar:sonar -Dsonar.organization=sheetalsonarcloud -Dsonar.projectKey=credit-service-1'

		
    		}
	 }

    stage("Quality Gate"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }

}
