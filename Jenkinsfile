node {
    def app

    stage('Clone repository') {
      

        checkout scm //this can be modified to checkout from a specific branch
    }

    stage('Update Configfile') { // 
            script {
                // BUILD_TRIGGER_BY_FULL = currentBuild.getBuildCauses()[0].shortDescription + " / " + currentBuild.getBuildCauses()[0].userId
                // BUILD_TRIGGER_BY_USER = currentBuild.getBuildCauses()[0].userId
                // BUILD_TRIGGER_BY_NAME = currentBuild.getBuildCauses()[0].shortDescription
                // echo "BUILD_TRIGGER_BY_FULL: ${BUILD_TRIGGER_BY_FULL}"
                // echo "BUILD_TRIGGER_BY_USER: ${BUILD_TRIGGER_BY_USER}"
                echo "BUILD_TRIGGER_BY: ${BUILD_TRIGGER_BY}"
                BUILD_TRIGGER_BY_NAME = sh(script:"echo "$BUILD_TRIGGER_BY"| awk -F 'by' '{print $NF}' | awk '{$1=$1};1'")
                echo "BUILD_TRIGGER_BY_NAME: ${BUILD_TRIGGER_BY_NAME}"
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
             sh "git config user.name ${BUILD_TRIGGER_BY_NAME}"
             sh "git add ."
             sh "git commit -m 'Trigger ${BUILD_TRIGGER_BY} Jenkins Job change_manifest: ${env.BUILD_NUMBER}'"
             sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/hello_node_deployment.git HEAD:main"
            }
      }


    }

}