node {
    def app

    
    stage('Clone repository') {
      checkout scm
    }

    
    stage('Build image') {
       app = docker.build("surakshaniveus/gitops")
    }

 

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
  
    
    stage('Clone repository') {
        script{
        
                 git credentialsId: 'github', url: 'https://github.com/suraksha-niveus/cicd-chart.git'
      
        }
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    
                        sh "git config user.email suraksha.shetty@niveussolutions.com"
                        sh "git config user.name suraksha-niveus"
                        sh "cat values.yaml"
                        sh "sed -i 's+tag.*+tag: ${BUILD_NUMBER}+g' values.yaml"
                        sh "cat values.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/cicd-chart.git HEAD:master"
      }
    }
  }
}
}
