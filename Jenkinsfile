pipeline {
    agent any
     tools{
         gradle "Gradle-6"
         nodejs "node"
     }
     post {
        success {
            emailext attachLog: true, 
                body:
                    """
                    <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>
                    <p>
                    View console output at 
                    "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"
                    </p> 
                      <p><i>(Build log is attached.)</i></p>
                    """,
                subject: "Status: 'SUCCESS' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'", 
                to: 'wanderobeth@gmail.com'
        }
        failure {
            emailext attachLog: true, 
                body:
                    """
                    <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>
                    <p>
                    View console output at 
                    "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"
                    </p> 
                      <p><i>(Build log is attached.)</i></p>
                    """,
                subject: "Status: FAILURE -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'", 
                to: 'wanderobeth@gmail.com'
        }
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
                sh 'npm install' 
            }
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
    post {
        success {
            notifyBuild('SUCCESSFUL')

            emailext attachLog: true,
                body: EMAIL_BODY,
                subject: EMAIL_SUBJECT_SUCCESS,
                to: EMAIL_RECEPIENT
        }

        failure {
            notifyBuild('FAILED')

            emailext attachLog: true,
               body: EMAIL_BODY ,
               subject: EMAIL_SUBJECT_FAILURE,
               to: EMAIL_RECEPIENT
        }
    }
}

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
    summary = "${subject} :aaw_yeah: (${env.BUILD_URL})"
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
    summary = """
    :sunglasses:
    ```
      ${subject}\n\n

      View console output here\n
      ${env.BUILD_URL}\n

      Site Link:\n
      https://gallery-pipeline.herokuapp.com\n

      Repo Link:\n
      https://github.com/Bwangari/gallery.git\n
    ```
    :v:
    """
  } else {
    color = 'RED'
    colorCode = '#FF0000'
    summary = "${subject} :coffin_dance: (${env.BUILD_URL})"
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}