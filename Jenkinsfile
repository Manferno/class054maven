pipeline {
    agent any

    stages {
        stage('Initialize'){
            steps{
                echo "Hola mami"
            }
        }
    }
      stage {
        stage('Build'){
            steps{
                sh 'mvn -B package'
            }
        }
    }
}