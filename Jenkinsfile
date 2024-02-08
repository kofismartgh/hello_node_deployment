node {
    def app

    stage('Clone repository') {
      

        checkout scm //this can be modified to checkout from a specific branch
    }

    stage('Update Configfile') { // 
            script {
                BUILD_TRIGGER_BY = currentBuild.getBuildCauses()[0].shortDescription + " / " + currentBuild.getBuildCauses()[0].userId
                echo "BUILD_TRIGGER_BY: ${BUILD_TRIGGER_BY}"
                BUILD_CAUSE_JSON = sh ( script: "curl --silent ${BUILD_URL}/api/json | tr '{}' '\n' | grep 'Started by'",returnStdout: true   ).trim()
                echo "BUILD_STARTED_BY:${BUILD_CAUSE_JSON}"
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        //sh "git switch master"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+image:  soloetzgh/agronode.*+image:  soloetzgh/agronode:${IMAGETAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
            }
    }

    stage('Commit Changes ') {
      script{
            withCredentials([usernamePassword(credentialsId: 'soloetzgit-kofismart', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
             sh "git config user.email solomon.martins@etranzact.com.gh"
             sh "git config user.name jenkins"
             sh "git add ."
             sh "git commit -m 'Trigger ${BUILD_TRIGGER_BY} Jenkins Job change_manifest: ${env.BUILD_NUMBER}'"
             sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/hello_node_deployment.git HEAD:main"
            }
      }


    }

}