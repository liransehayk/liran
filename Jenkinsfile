node {
    stage('checkout') {
        checkout scm
    }

    stage('Example') {
        echo "Current build number: ${currentBuild.number}"
        writeFile file: 'index.html', text: "Current build number: ${currentBuild.number}"
        sh 'ls -ltr'
        docker.withRegistry('', 'dockerCreds') {
            def myImage = docker.build("liransehayk/nginx-exam:${currentBuild.number}")
            
            myImage.push('latest')
        }
    }

    stage('kubernetes') {
        withKubeConfig([credentialsId: 'liran-cluster']) {  
            sh 'kubectl apply -f deployment.yaml'  
     }  
    }
}