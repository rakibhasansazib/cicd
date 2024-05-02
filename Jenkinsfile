node {
    def app

    stage('Clone repository'){

        checkout scm
    }

    stage('Build Docker image'){

        app = docker.build("192.168.50.56:9091/argocd-dev/devops:${env.BUILD_NUMBER}")

    }

    stage('Test Docker image'){

        app.inside {
            sh 'echo "Tests passed"'
        }

    } 

    stage('Push image to Nexus'){

        sh 'docker login -u admin -p admin http://192.168.50.56:9091/repository/argocd-dev/'
        app.push("${env.BUILD_NUMBER}")

    }

    stage('Trigger Update Manifest'){

        echo "triggering Update manifest Job"
            build job: 'argocd-update-menifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]

    }
}
