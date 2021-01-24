node {
    stage('checkout') {
        checkout scm
    }

    stage('Build') {
        writeFile file: 'index.html', text: "Hello visitor!: ${BUILD_NUMBER}"        
        docker.withRegistry('', 'dockerCreds') {
            def myImage = docker.build("liransehayk/nginx-exam:${BUILD_NUMBER}")
            
            myImage.push("${BUILD_NUMBER}")
        }
    }

    stage('Deploy') {
        sh 'sed s/tag/${BUILD_NUMBER}/ deployment.yaml.template > deployment.yaml'
        withKubeConfig([credentialsId: 'kubeconfig']) {  
            sh 'kubectl apply -f deployment.yaml'  
     }  
    }
}