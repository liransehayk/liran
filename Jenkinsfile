node {
    stage('checkout') {
        checkout scm
    }

    stage('Example') {
        echo "Current build number: ${BUILD_NUMBER}"
        writeFile file: 'index.html', text: "Hello visitor!: ${BUILD_NUMBER}"        
        docker.withRegistry('', 'dockerCreds') {
            def myImage = docker.build("liransehayk/nginx-exam:${BUILD_NUMBER}")
            
            myImage.push("${BUILD_NUMBER}")
        }
    }

    stage('kubernetes') {
        sh 'sed s/tag/${BUILD_NUMBER}/ deployment.yaml.template > deployment.yaml'
        withKubeConfig([credentialsId: 'kubeconfig']) {  
            sh 'kubectl apply -f deployment.yaml'  
     }  
    }
}