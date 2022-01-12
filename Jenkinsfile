node {
   
     
//    stage('git clone') {
  //      git credentialsId: 'Private-key', url: 'git@bitbucket.org:manilnt/k8s-jenkins-aws-eks.git' 
//    }
 //   try {
    // do your job logic
   // } catch (Exception e) {
     //bitbucketStatusNotify(buildState: 'FAILED')
//}

    stage('Git clone') {
       git 'https://github.com/mani7444/K8-Jenkins-AWS.git'
    }
    
    stage ('Gitleaks Scan') {
					sh 'gitleaks detect --report-path gitleaks-report.json --verbose'
				}
    
  //  stage('Compile-Package'){
      //def mvnHome =  tool name: 'maven3', type: 'maven'   
      //sh "${mvnHome}/bin/mvn clean package"
   //}
     stage('Build Package') {

       sh './gradlew build'

    }
    

   
    stage('SemGrep Scan') {
			sh 'semgrep --config=auto $(pwd)'
		}
   
    stage('OWASP Dependency Check') {
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
            
  // stage('SonarQube analysis') {
//    def scannerHome = tool 'Sonarqube Scanner';
  //  withSonarQubeEnv('SonarInstallation') { // If you have configured more than one global server connection, you can specify its name
    //  sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
      
//    }
  // }
    
     
    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t jhooq-docker-demo .'
        sh 'docker image list'
        sh 'docker tag jhooq-docker-demo mani7444/jhooq-docker-demo:jhooq-docker-demo'
    }
     stage('trivy'){
         sh 'trivy image jhooq-docker-demo'
     }
     

    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) { 
    }
    

    stage("Push Image to Docker Hub"){
        sh 'docker push  mani7444/jhooq-docker-demo:jhooq-docker-demo'
    }
        
       
    stage('Docker Image Push') {
        withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
        sh "docker login -u mani7444 -p ${dockerPassword}"
    }
        sh 'docker push mani7444/demo-app'
    }
   // stage("kubernetes deployment"){
     //   sh 'kubectl apply -f k8s-spring-boot-deployment.yml'
        
//}
   // stage ('Kubescape Scan') {
    //sh 'curl -s https://raw.githubusercontent.com/armosec/kubescape/master/install.sh | /bin/bash'    
	//sh 'kubescape scan --submit'
	//}
   
  //  stage ('ZAP') {
    //    sh 'docker pull owasp/zap2docker-weekly'
      //  sh 'docker run -i owasp/zap2docker-stable zap-baseline.py -t "http://ae0c2747619d241bf8e1eac5ca7bc9a9-380498603.us-east-2.elb.amazonaws.com/hello" -l INFO'
       // sh 'zap-baseline.py -t http://ae0c2747619d241bf8e1eac5ca7bc9a9-380498603.us-east-2.elb.amazonaws.com/hello -r zap_baseline_report.html -l '
//}
    //stage('report') { 
        //cucumber fileIncludePattern: '"/".json' sortingMethod 'ALPHABETICAL'
    //    cucumber buildStatus: 'null', customCssFiles: '', customJsFiles: '', failedFeaturesNumber: 1, failedScenariosNumber: 1, failedStepsNumber: 1, fileIncludePattern: '**/*.json', pendingStepsNumber: 1, skippedStepsNumber: 1, sortingMethod: 'ALPHABETICAL', undefinedStepsNumber: 1
      //  }
      
      
}

    



