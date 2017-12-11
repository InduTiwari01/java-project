properties([pipelineTriggers([githubPush()])])

node('linux') {   
	stage('Unit Tests') {    
		git 'https://github.com/InduTiwari01/java-project.git'
		sh 'ant -f test.xml -v' 
		junit 'reports/result.xml'
	}   
	stage('Build') {    
		sh 'ant -f build.xml -v'   
	} 
	stage('Deploy') {    
		sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://jenkins-s3bucket-1elppjd5g7ims'   
	}
	stage('Report') {   
		withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '1640739a-0e29-4a5b-8998-39dcec61766d', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    		sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
	}
	}
}
