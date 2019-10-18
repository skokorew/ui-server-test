node {
    stage ('Checkout') {
        checkout scm
    }

    stage ('Run container') {
        sh 'docker run --name ui-service -d -p 8689:8080 scrumtrek/ui-server:latest'
    }

    stage ('Run UAT') {
        //docker.image('scrumtrek/ui-server:latest').withRun('--name ui-service -p 8689:8080') { c ->
            withMaven(maven: 'local_mvn') {
                        sh 'mvn clean install -P local'
            }
        //}
    }

    stage ('Stop container') {
        sh 'docker stop ui-service'
        sh 'docker rm -f ui-service'
    }

    stage ('Test') {
        junit allowEmptyResults: true, testResults:'target/surefire-reports/*.xml'
    }
}