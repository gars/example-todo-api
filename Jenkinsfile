def status = 'inicial'
node('php'){
    stage('Clean'){
        deleteDir()
        sh 'ls -la'
    }
    
    stage('Fetch') {
        checkout scm
    }
    
    stage('Build'){
        sh 'composer install --prefer-dist --no-dev --ignore-platform-reqs'
        status = 'alterado no build'
    }
    
    stage('Config') {
        parallel(
            'config cache': {
                sh 'php artisan config:cache'                
            },
            'config route': {
                sh 'php artisan'
            }
        )
    }
    
    stage('Docker Build') {
        sh 'docker build -t gars/todoapi:$BUILD_NUMBER .'
    }
    
    stage('Docker Ship') {
        sh 'docker push gars/todoapi:$BUILD_NUMBER'
    }
}
sh 'echo status'
