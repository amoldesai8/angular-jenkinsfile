node {

      stage('Deploy') {
          
        withAWS(credentials: 'awsCreds', region: 'ap-south-1') {
            
            s3Delete bucket: 's3jenkinstesting', path: '/'
            
    // some block
                }

      } 

}
