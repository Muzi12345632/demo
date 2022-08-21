pipeline { 
    agent any 
    options {
        skipStagesAfterUnstable()
    }
    environment { 
        CC = 'clang'
    }
    stages {
        stage('Build') { 
            steps { 
                sh 'make' 
                // saves files to jenkinsfile controller for retrieving later
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                echo $CC
                
            }
        }
        stage('Test'){
            steps {
                sh 'make check'
                junit 'reports/**/*.xml' 
            }
        }
        stage('Deploy') {
            //deploy if previous stage passed
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
                sh 'make publish'
            }
        }
    }
}
