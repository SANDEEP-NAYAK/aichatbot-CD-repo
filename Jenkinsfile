pipeline {
  agent none

  stages {
    
    stage('Clone Code') {
      agent {
        node {
          label 'aws-eks'
         }
      }

      steps {
        echo "Cloning the code"
        git url:'https://github.com/SANDEEP-NAYAK/aichatbot-CD-repo.git', branch: 'main'
      }
    }
    
    stage('Update Mainfest File in GIT') {
      agent {
        node {
          label 'aws-eks'
         }
      }

      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
            sh "git config user.email wd.sandeepnayak@gmail.com"
            sh "git config user.name SANDEEP-NAYAK"
            // sh "git remote add origin https://github.com/SANDEEP-NAYAK/aichatbot-CD-repo.git"
            sh "cat deploymentService.yaml"
            sh "sed -i 's+localhost:6666/aichatbot.*+localhost:6666/aichatbot:${DOCKERTAG}+g' deploymentService.yaml"
            sh "cat deploymentService.yaml"
            sh "git add ."
            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
            sh "git push origin main"
         }
       }
     }
   }
  }
}
