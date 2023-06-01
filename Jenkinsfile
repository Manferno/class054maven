pipeline {
    agent any

    stages {
        stage('Initialize'){
            steps{
                echo "Hola mami"
            }
        }
    }
        stage('Build'){
            steps{
                sh 'mvn -B package'
            }
        }
    }
