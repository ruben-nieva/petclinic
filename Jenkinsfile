node ('dockerslave'){
    
    stage 'Checkout'
    git url: "https://github.com/ruben-nieva/petclinic.git", branch: 'master'

    stage 'Build docker image'
    // Build petclinic in a Maven3+JDK8 Docker container
    docker.image('maven:3-jdk-8').inside('-v /.m2:/root/.m2') {
        sh 'mvn -B package -DskipTests'
    }

    stage 'Build application Docker image'
    def appImg = docker.build("nicolas-deloof/petclinic")

    stage 'Push to GCR'
    docker.withRegistry('http://10.2.2.2:2375') {
        appImg.push();
    }

    //stage 'Run app on Kubernetes'
    //withKubernetes( serverUrl: 'https://10.2.2.2:8443', credentialsId: 'admin' ) {
    //      sh 'kubectl run petclinic --image=gcr.io/nicolas-deloof/petclinic --port=8080'
   // }

    // ... Do some tests on deployed application web UI

}
