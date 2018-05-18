node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("sudipta007/jboss-docker")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credential') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    
   stage('Deploy image') {
       AWS_SECRET = AKIAJCW3GX2MXFOWJOTA
       AWS_REGION = OJayIXfogAjCD6G/bM+ydj93lO8lRx3rdP6ilf/J
       sh ecs-deploy -k $AWS_KEY -s $AWS_SECRET -r $AWS_REGION -c default -n sample-webapp -i sudipta007/jboss-docker:latest
       
   }
    
}
