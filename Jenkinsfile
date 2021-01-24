node {
    stage('checkout') {
        checkout scm
    }

    stage('Example') {
        echo "Current build number: ${currentBuild.number}"
        writeFile file: 'index.html', text: "Hello visitor!: ${currentBuild.number}"
        sh 'ls -ltr'
        docker.withRegistry('', 'dockerCreds') {
            def myImage = docker.build("liransehayk/nginx-exam:${currentBuild.number}")
            
            myImage.push('latest')
        }
    }

    stage('kubernetes') {
        withKubeConfig([credentialsId: 'kubeconfig']) {  
            sh 'kubectl apply -f deployment.yaml'  
     }  
    }
}