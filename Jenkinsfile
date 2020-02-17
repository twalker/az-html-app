pipeline {
   agent {
     docker {
       image 'node:lts'
     }
   }

   stages {
      stage('Environment') {
        steps {
          echo sh('node --version')
        }
      }
      stage('Install') {
        steps {
         sh 'npm install --production'
        }
      }

      stage('Build') {
         steps {
          sh 'npm run build'
         }
      }
      stage('CreateDeployment') {
         steps {
          //zip zipFile: 'built-html.zip', archive: true, dir: '../' // works
          // Create a zip file of content in the workspace
          zip dir: "${workspace}/build", zipFile: "built-html-${env.BUILD_ID}.zip", archive: true
          // archiveArtifacts artifacts: "built-html-${env.BUILD_ID}.zip", fingerprint: true
         }
      }
   }
}