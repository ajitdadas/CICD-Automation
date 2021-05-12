pipeline {
	    agent any
		    environment {
	        MAJOR = '1'
	        MINOR = '0'
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
	        UIPATH_ORCH_LOGICAL_NAME = "manoj_vermaa"
	        UIPATH_ORCH_TENANT_NAME = "manoj_vermaa"
	        UIPATH_ORCH_FOLDER_NAME_UAT = "default"
	        UIPATH_ORCH_FOLDER_NAME_PROD = "default"
	    }
	

	    stages {
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	            }
	        }
	
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false
	        		)
	            }
	        }
	        
	        stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'
	            }
	        }
	
	        stage('Deploy to UAT') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to UAT "
	                UiPathDeploy (
	                	packagePath: "Output\\${env.BUILD_NUMBER}",
	                	orchestratorAddress: "${UIPATH_ORCH_URL}",
	                	orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                	folderName: "${UIPATH_ORCH_FOLDER_NAME_UAT}",
	                	environments: 'PROD',
	                	credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: '8DEv1AMNXczW3y4U15LL3jYf62jK93n5'), 
	        		)
	            }
	        }
	
		    stage('Deploy to Production') {
	            steps {
	                echo 'Deploy to Production'
	                UiPathDeploy (
	                	packagePath: "Output\\${env.BUILD_NUMBER}",
	                	orchestratorAddress: "${UIPATH_ORCH_URL}",
	                	orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                	folderName: "${UIPATH_ORCH_FOLDER_NAME_PROD}",
	                	environments: 'PROD',
	                	credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: '8DEv1AMNXczW3y4U15LL3jYf62jK93n5'), 
	        		)
	           	}
	       	}
	    }

	    options {
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            cleanWs()
	        }
	    }
	}
