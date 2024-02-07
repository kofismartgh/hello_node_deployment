node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Update GIT') {
            script {
                //catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'soloetzgit-kofismart', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email solomon.martins@etranzact.com.gh"
                        sh "git config user.name Jenkins"
                        //sh "git switch master"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+image:  soloetzgh/agronode.*+image:  soloetzgh/agronode:${IMAGETAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/hello_node_deployment.git HEAD:main"
      }
    //}
  }
}
}