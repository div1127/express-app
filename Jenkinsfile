#!/bin/groovy
pipeline {
    agent any
    environment {
        def imagetag="066089857847.dkr.ecr.us-west-2.amazonaws.com/expressapp:${BUILD_NUMBER}"
        def taskDefile="file://aws/task-definition-${imagetag}.json"
        def message=sh (script: 'git log -1 --pretty=%B',returnStdout: true)
        def JIRA_SITE="celestialsys-tk1"
       }
    stages {
        stage('Build') {
            steps {
                echo "Building..."
                sh "npm install"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'npm test'
                echo "Starting end to end testing"
                sh "npm run test:e2e"
            }
        }
      stage('Submit a PR') {          
            steps {                
                script {                    

                    echo "submitting PR to dev..."
                    sh "/usr/local/bin/hub pull-request -b development -m TIS-10"
                   
                    
                }
                
            }
       
        }
       
        stage('JIRA') {
           steps{
                 jiraNewIssue issue: [fields: [project: [key: 'TIS'],summary: 'New JIRA Created from Jenkins.',description: 'New JIRA Created from Jenkins.', issuetype: [id: '10004']]], site: 'celestialsys-tk1'
               }	
   }
}
}
