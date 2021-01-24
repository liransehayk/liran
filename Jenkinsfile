node {
    stage('Example') {
        echo "Current build number: ${currentBuild.number}"
        writeFile file: 'index.html', text: "Current build number: ${currentBuild.number}"
        docker.withRegistry('', 'dockerCreds') {
            def myImage = docker.build("liransehayk/nginx-exam:${currentBuild.number}")
            
            myImage.push('latest')
        }
    }
}