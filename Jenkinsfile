node {
    stage ('Checkout') {
        checkout scm
    }

    stage ('Run UAT') {
        docker.image('scrumtrek/ui-server:latest').withRun('--name ui-service -p 8689:8080') { c ->
            withMaven(maven: 'local_mvn') {
                sh 'mvn clean install -P local'
            }
        }
    }

    stage ('Test') {
        junit allowEmptyResults: true, testResults:'target/surefire-reports/*.xml'
    }
}