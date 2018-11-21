
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
                
                echo "Building.. "
                echo "branch: ${env.BRANCH_NAME}"
                sh "mvn clean install -DskipTests"
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
                sh "docker build -t 892943703739.dkr.ecr.us-west-2.amazonaws.com/samplewarfilerepo:latest ."
                sh "eval \$(aws ecr get-login --no-include-email  --region us-west-2)"
                sh  "docker push 892943703739.dkr.ecr.us-west-2.amazonaws.com/samplewarfilerepo:latest"
               }
            }
    
         stage('Deploy') {
          steps {
                   sh "aws ecs list-container-instances --cluster devenv --region us-west-2"
                  // sh "aws ecs describe-container-instances --cluster devenv --container-instances 8d1208ca-53ab-4e76-8ceb-e4a1c637b179 --region us-west-2"
                    sh "aws ecs register-task-definition --cli-input-json file://sample.json --region us-west-2"
                  // sh "aws ecs list-task-definitions --region us-west-2"
                   sh "aws ecs run-task --cluster devenv --task-definition sleep360:1 --count 1 --region us-west-2"
                   //sh "aws ecs list-tasks --cluster default --region us-west-2" 
                   //sh "aws ecs describe-tasks --cluster default --task 4831f0e4-c76b-45aa-8a96-952c4341d749 --region us-west-2" 
                   sh "aws ecs create-service --cluster default --service-name nbcsampleservice --task-definition sleep360:5 --desired-count 10 --region us-west-2"
                   //sh "aws ecs update-service --cluster default --service nbcsampleservice --task-definition sleep360:5 --desired-count 10 --region us-west-2"
          }
        }
        }
}
