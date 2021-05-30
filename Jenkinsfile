
pipeline {
agent {
    node {
        label 'master'
    }
}

    stages {
        stage ('Build sa-frontend') {
            steps {
                sh 'cd ${WORKSPACE}/sa-frontend && npm install && npm run build'
            }
        }
        stage ('Build sa-webapp') {
            steps {
                sh 'cd ${WORKSPACE}/sa-webapp && mvn install'
            }
        }
        stage ('Build Docker sa-frontend') {
            steps {
                sh 'cd ${WORKSPACE}/sa-frontend && docker build -t sa-frontend'
            }
        }
        stage ('Build Docker sa-webapp') {
            steps {
                sh 'cd ${WORKSPACE}/sa-webapp &&  docker build -t sa-webapp'
            }
        }
        stage ('Build Docker sa-logic') {
            steps {
                sh 'cd ${WORKSPACE}/sa-logic &&  docker build -t sa-logic'
            }
        }
        stage ('Push to registry') {
            steps {  
                sh 'docker tag sa-frontend shikshagarg/sa-frontend'
                sh 'docker tag sa-webapp shikshagarg/sa-webapp'  
                sh 'docker tag sa-logic shikshagarg/sa-logic'
                sh 'docker push shikshagarg/sa-frontend'
                sh 'docker push shikshagarg/sa-wepapp'
                sh 'docker push shikshagarg/sa-logic'
             }
        }
        stage('Deploy to kubernetes') {
            steps {
                sh 'cd ${WORKSPACE}/resource-manifests && kubectl apply -f .'
            }
        }
        stage('Archieve Artifacts') {
            steps {
                archieveArtifacts '**/*.jar'
            }
        }
        stage('Email Notification') {
            steps {
                emailext body: 'Please check', subject: 'Build Failed', to: 'shiksha3010@gmail.com'
            }
        }
    
    }
}
