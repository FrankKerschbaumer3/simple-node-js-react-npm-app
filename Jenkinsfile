pipeline{
    agent{
        label "NodeAMI"
    }
    //tools{
    //    nodejs "NodeJs" 
    //}
    //options {
    //  disableConcurrentBuilds()
    //  buildDiscarder(logRotator(numToKeepStr: '3'))
    //  retry(3)
    //}
    stages{
        stage('Build Test-branch') {
            when { branch 'testbranch' }
            steps {
                script {
                    echo("Building from PR")
                    sh 'npm i'
                    sh 'npm i -g lerna'
                }
            }
        }
        stage('Build-master') {
            when { branch 'master' }
            steps {
                echo("Building dock docker image and pushing to registry.");
                script {
                    sh 'docker build -t dev .'
                    sh 'df -ih'
                }
            }
        }
        stage('Build-dev') {
            when { branch 'dev' }
            steps {
                echo("Building dock docker image and pushing to registry.");
                script {
                    sh 'docker build -t dev .'
                    sh 'df -ih'
                }
            }
        }
    }
    post{
        success {
           sh 'docker ps -a'
           sh 'docker images'
           sh 'df -hi'
           sh 'whoami'
           sh 'pwd'
        }
        //failure {
        //    sh 'docker system prune -a -f'
        // }
        cleanup {
            sh 'docker system prune -a -f'
        }
    }
} 
