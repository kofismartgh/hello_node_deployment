node {
    def app
    wrap([$class: 'BuildUser']) {
    def user = env.BUILD_USER_ID
    }

    stage('Clone repository') {
      

        checkout scm //this can be modified to checkout from a specific branch
    }

    stage('Update Configfile') { // 
            script {
                BUILD_TRIGGER_BY = currentBuild.getBuildCauses()[0].shortDescription + " / " + currentBuild.getBuildCauses()[0].userId
                echo "BUILD_TRIGGER_BY: ${BUILD_TRIGGER_BY}"
                echo "BUILD_USER_ID_1:${user}"
                echo "BUILD_USER:${env.BUILD_USER}"
                echo "BUILD_FNAME:${env.BUILD_USER_FIRST_NAME}"
                echo "BUILD_USER_ID:${env.BUILD_USER_ID}"
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