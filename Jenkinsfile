pipeline {
    agent any
    
    stages {
        stage('Compile') {
            steps {
                gradlew('clean', 'classes')
            }
        }
        stage('Unit Tests') {
            steps {
                gradlew('test')
            }
            post {
                always {
                    junit '**/build/test-results/test/TEST-*.xml'
                }
            }
        }
  
        stage('Assemble') {
            steps {
                gradlew('assemble')
                stash includes: '**/build/libs/*.war', name: 'app'
            }
        }
        stage('Promotion') {
            steps {
                timeout(time: 1, unit:'DAYS') {
                    input 'Subirlo a Produccion?'
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                unstash 'app'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/*.war.*', onlyIfSuccessful: true
        }
    }    
}

def gradlew(String... args) {
    sh "./gradlew ${args.join(' ')} -s"
}
