pipeline {
    agent any
     tools{
         gradle "Gradle-6"
     }
     environment{
         NEW_VERSION ='1.3.0'
     }

    stages {
        stage('clone repository'){
             steps{
                 git 'https://github.com/Bwangari/gallery.git'
             }
         }
        stage('Build') {
            steps {
                echo 'Building..'
                echo "building version ${New_VERSION}"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}