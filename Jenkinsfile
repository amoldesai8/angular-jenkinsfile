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
          
        withAWS(credentials: 'awsCreds', region: 'ap-south-1') {
            
            s3Delete bucket: 's3jenkinstesting', path: '**/*'
            s3Upload acl: 'Private', bucket: 's3jenkinstesting', cacheControl: '', excludePathPattern: '', includePathPattern: '**/*', metadatas: [''], redirectLocation: '', sseAlgorithm: '', text: '', workingDir: 'dist'
    // some block
                }

      } 

}
