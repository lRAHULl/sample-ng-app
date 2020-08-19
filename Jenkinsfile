node('jenkins-angular') {
    stage('checkout') {
        git branch: "${gitBranch}", url: "${gitRepoName}"
    }
  
    stage('Build') {
        container('nodejs') {
            sh "npm install"
            sh "npm run build"
            
            sh 'ls dist/sample-ng-app'
        }
    }
}
