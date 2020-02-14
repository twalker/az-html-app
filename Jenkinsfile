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
      stage('Build') {
         steps {
            echo 'Node version'
            echo sh('node --version')
         }
      }
   }
}