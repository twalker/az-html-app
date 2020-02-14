pipeline {
   agent {
     docker {
       image 'node:lts'
     }
   }

   stages {
      stage('Hello') {
         steps {
            echo 'Hello World'
            echo sh('pwd')
         }
      }
      stage('Version') {
         steps {
            echo sh('node --version')
         }
      }
   }
}