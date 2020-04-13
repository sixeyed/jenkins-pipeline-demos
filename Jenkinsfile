pipeline {
   agent any
   
   environment {
       DEMO='1.3'
   }

   stages {
      stage('stage-1') {
         steps {
            echo "This is build number $BUILD_NUMBER of demo $DEMO"
            sh '''
               echo "Using a multi-line shell step"
               chmod +x test.sh
               ./test.sh
            '''
         }
      }
   }
}