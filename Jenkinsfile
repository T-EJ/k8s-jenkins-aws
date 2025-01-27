node {

    stage("Git Clone"){

       git branch: 'main', url: 'https://github.com/T-EJ/k8s-jenkins-aws/'
    }

     stage('Gradle Build') {
       sh 'chmod +x gradlew'

       sh './gradlew build'

    }

    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t jhooq-docker-demo .'
        sh 'docker image list'
        sh 'docker tag jhooq-docker-demo tejmt/jhooq-docker-demo:jhooq-docker-demo'
    }

    withCredentials([string(credentialsId: 'Dockerhub', variable: 'PASSWORD')]) {
        sh 'docker login -u tejmt -p $PASSWORD'
    }

    stage("Push Image to Docker Hub"){
        sh 'docker push  tejmt/jhooq-docker-demo:jhooq-docker-demo'
    }
    
    stage("kubernetes deployment"){
        sh 'kubectl apply -f k8s-spring-boot-deployment.yml'
    }
} 
