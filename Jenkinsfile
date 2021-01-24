node {
    stage('checkout') {
        checkout scm
    }

    stage('Example') {
        echo "Current build number: ${currentBuild.number}"
        writeFile file: 'index.html', text: "Hello visitor!: ${currentBuild.number}"        
        docker.withRegistry('', 'dockerCreds') {
            def myImage = docker.build("liransehayk/nginx-exam:${currentBuild.number}")
            
            myImage.push('latest')
        }
    }

    stage('kubernetes') {
        sh 'sed s/<tag>/${currentBuild.number}/ deployment.yaml.template > deployment.yaml'
        withKubeConfig([credentialsId: 'kubeconfig']) {  
            sh 'kubectl apply -f deployment.yaml'  
     }  
    }
}