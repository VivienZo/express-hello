#!groovy

node('node') {


    currentBuild.result = "SUCCESS"

    try {

       stage('Checkout'){

          checkout scm
       }

       stage('Cleanup'){

         echo 'prune and cleanup'
         sh 'npm prune'
         sh 'rm node_modules -rf'
         echo 'node modules cleaned'
         
       }
       
       stage('Test'){

         env.NODE_ENV = "test"

         print "Environment will be : ${env.NODE_ENV}"

         sh 'node -v'
         sh 'npm prune'
         sh 'npm install'
         sh 'npm test'

       }

       stage('Run'){
       
            env.NODE_ENV = "production"
            print "Environment will be : ${env.NODE_ENV}"

            sh 'npm start'
       }


    }
    catch (err) {

        currentBuild.result = "FAILURE"
        
        echo 'Catched error'
        print "err : ${err}"
        
        throw err
    }

}
