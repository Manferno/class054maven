pipeline {
    agent any

    stages {
        stage('Initialize'){
            steps{
                echo "Hola mami"
            }
        }
    }

      stages {
        stage('Build'){
            steps{
                sh 'mvn -B package'
            }
        }
    }

}