pipeline {
    agent any
    stages{
        stage('Build') {
            steps {
                bat 'mvn -B -DskipTests clean package  --fail-never'
            }
        }
        stage('Doc') {
            steps {
                bat 'mvn javadoc:jar --fail-never'
            }
        }
        stage('pmd') {
            steps {
                bat 'mvn pmd:pmd --fail-never'
            }
        }
        stage('Test report') {
            steps {
                bat 'mvn surefire-report:report  --fail-never'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
        }
    }
}
