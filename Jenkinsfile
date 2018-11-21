
pipeline {
    agent any
    tools {
           maven 'mvn'
          }
    
    stages {
      stage ('Initialize') {
            steps {
                     sh '''
                         echo "PATH = ${PATH}"
                         echo "M2_HOME = ${M2_HOME}"
                     '''
                  }
        }
       stage('Build') {
            steps {
                script {
                echo "Building.. "
                echo "branch: ${env.BRANCH_NAME}"
                sh "mvn clean install -DskipTests"
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                sh "docker build -t 892943703739.dkr.ecr.us-west-2.amazonaws.com/varshaapachetomcat:latest ."
                sh "eval \$(aws ecr get-login --no-include-email  --region us-west-2)"
               }
            }
        }
     
    }
}
