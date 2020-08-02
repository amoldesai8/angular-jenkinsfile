#!groovy


node {
    stage('Checkout') {
        //disable to recycle workspace data to save time/bandwidth
        deleteDir()
        checkout scm

        //enable for commit id in build number
        //env.git_commit_id = sh returnStdout: true, script: 'git rev-parse HEAD'
        //env.git_commit_id_short = env.git_commit_id.take(7)
        //currentBuild.displayName = "#${currentBuild.number}-${env.git_commit_id_short}"
    }

    stage('NPM Install') {
        withEnv(["NPM_CONFIG_LOGLEVEL=warn"]) {
            bat 'npm install'
        }
    }

    stage('Build') {
        bat 'npm install -g @angular/cli'
        bat 'npm run build --prod'
    }
      stage('Deploy') {
          
          withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'awsCreds', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
          {
              s3Delete(bucket: 's3jenkinstesting', path:'**/*')
              s3Upload(bucket: 's3jenkinstesting', workingDir:'build', includePathPattern:'**/*');
            }

      } 

}
