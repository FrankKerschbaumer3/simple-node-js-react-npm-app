pipeline{
    agent{
        label "NodeAMI"
    }
    tools{
        nodejs "NodeJs" 
    }
    //options {
    //  disableConcurrentBuilds()
    //  buildDiscarder(logRotator(numToKeepStr: '3'))
    //  retry(3)
    //}
    stages{
        stage('Build PR') {
            when { changeRequest() }
            steps {
                script {
                    echo("Building from PR")
                    sh 'npm i'
                    sh 'npm i -g lerna'
                }
            }
        }
        stage('Build-Docker') {
            when { branch 'master' }
            steps {
                echo("Building dock docker image and pushing to registry.");
                script {
                    sh 'docker build .'
                }
            }
        }
    }
    post{
        success {
           sh 'docker ps -a'
           sh 'docker images'
        }
        failure {
            sh 'docker system prune -a -f'
        }
        //cleanup {
        //    sh 'docker system prune -a -f'
        //}
    }
} 
