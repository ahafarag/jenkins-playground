pipeline {
    agent any
    environment {
        MIN_SIZE = 0
        MAX_SIZE = 100
    }

    stages {
        stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                            choice(
                                choices: ['DEVELOPMENT','STAGING', 'PRODUCTION'], 
                                name: 'ENVIRONMENT',
                                description: 'Choose the deployment environment.'
                            ),
                            booleanParam(
                                defaultValue: true, 
                                description: '', 
                                name: 'BOOLEAN'
                            ),
                            text(
                                defaultValue: '''
                                this is a multi-line 
                                string parameter example
                                ''', 
                                 name: 'MULTI-LINE-STRING'
                            ),
                            string(
                                defaultValue: 'scriptcrunch', 
                                name: 'STRING-PARAMETER', 
                                trim: true
                            )
                        ])
                    ])
                }
            }
        }        
        stage('GET SCM') {
            steps {
                echo 'GET SCM'
            }
        }    
        
        stage('Build') {
            steps {
                echo 'Build'
            } 
        }
        stage('test') {
            steps {
                echo 'test'
            }
        }  
        stage('deployment to non-production') {
            when {
              expression { params.ENVIRONMENT != "PRODUCTION" }   
            }
            steps {
                echo 'deployint to ${params.ENVIRONMENT}'
            }
                             
        }
        stage('deployment to production') {
            when {
              expression { params.ENVIRONMENT == "PRODUCTION" }   
            }
            steps {
                input message: 'Confirm deploying to production ...', ok: 'Deploy'
                echo 'deployint to ${params.ENVIRONMENT}'
            }
                             
        }        
        stage('report') {
            steps {
                sh 'echo "This is a report for deployment" > report.txt '
                archiveArtifacts allowEmptyArchive: true, artifacts: '*.txt', fingerprint: true, followSymlinks: false, onlyIfSuccessful: true
            }
        }
        stage('Default: Scale') {
            steps {
                echo "MIN_SIZE={$env.MIN_SIZE}"
                echo "MAX_SIZE={$env.MAX_SIZE}"
            }                 
        }  
        stage('Scale up by 10x') {
            environment {
                MIN_SIZE = 10
                MAX_SIZE = 100
            }            
            steps {
                echo "MIN_SIZE={$env.MIN_SIZE}"
                echo "MAX_SIZE={$env.MAX_SIZE}"
            }                 
        }         
    }
}
