#!/usr/bin/env groovy
//comment
node ('docker&&linux'){
    checkoutFromRepo('main', 'https://github.com/mzapatal2/cloudcampJenkinsCICD.git', 'git-credentials')

    buildDockerfile("hello-world-python:latest")
    //pushDockerImage()
}

// Metodos
def checkoutFromRepo(branch, repoURL, credentialsID)
{
    stage('Checkout') {
        checkout scmGit(
                        branches: 
                        [[name: branch]], 
                        extensions: [], 
                        userRemoteConfigs: [[url: repoURL, credentialsId: credentialsID]]
                        )
    }
}

def buildDockerfile(tag, context=".", fileArg="")
{
    if (fileArg=="")
    {
        path=""
    }
    else{
        path="-f ${fileArg}"
    }

    stage ('Build') {
        sh "exec docker build -t ${tag} ${path} ${context}"
    }
}

def pushDockerImage()
{
    stage ('Push') {
        sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 851725481871.dkr.ecr.us-east-1.amazonaws.com'
        sh 'docker tag hello-world-python:latest 851725481871.dkr.ecr.us-east-1.amazonaws.com/hello-world-python:1.0.0-beta.2'
        sh 'docker tag hello-world-python:latest 851725481871.dkr.ecr.us-east-1.amazonaws.com/hello-world-python:latest'
        sh 'docker push --all-tags 851725481871.dkr.ecr.us-east-1.amazonaws.com/hello-world-python'
        
    }
}