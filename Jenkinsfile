node('jenkins-angular') {
    stage('Build') {
        container('nodejs') {
            sh "npm install"
            sh "npm run build"
            
            sh 'ls dist/sample-ng-app'
        }
    }
}
