node(){
    
    
def containerName="nodejs"
def tag="latest"
def dockerHubUser="swatig139627"
    
    stage('prepare enviroment'){
      
        tagName="latest"
    }
    
    stage('git code checkout'){
        try{
            echo 'checkout the code from git repository'
            git 'https://github.com/SiyaaJhawar/ReactJSApplication.git'
        }
        catch(Exception e){
            echo 'Exception occured in Git Code Checkout Stage'
            currentBuild.result = "FAILURE"
            emailext body: '''Dear All,
            The Jenkins job ${JOB_NAME} has been failed. Request you to please have a look at it immediately by clicking on the below link. 
            ${BUILD_URL}''', subject: 'Job ${JOB_NAME} ${BUILD_NUMBER} is failed', to: 'swatig139@gmail.com'
        }
    }
    
    stage('Build the Application'){
        echo "Cleaning... Compiling...Testing... Packaging..."
        //sh 'mvn clean package'
        sh "npm install"  
        sh "npm run build"
    }
    stage('Image Build'){
        sh "docker build -t $containerName:$tag --pull --no-cache ."
        echo "Image build complete"
    }
    

    stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
            sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
            
        }
}
      stage('kubernetes deploy')
    {
        sh "kubectl get pods -o wide"
        sh "kubectl apply -f deployment.yaml"
        sh "kubectl get deployments"
        sh "kubectl apply -f service.yaml"
        sh "kubectl get services"
}


    
    
}
