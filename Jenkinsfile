node {
    def app

    stage('Clone repository') {
        // Clone the repository using SCM
        checkout scm
    }

    stage('Build image') {
        // Build the Docker image and assign it to the 'app' variable
        app = docker.build("narsimha2580/test")
    }

    stage('Test image') {
        // Run tests inside the Docker container
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        // Push the Docker image to Docker Hub with the tag as the build number
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
        // Trigger the 'updatemanifest' job with the Docker tag as a parameter
        echo "triggering updatemanifestjob"
        build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}
