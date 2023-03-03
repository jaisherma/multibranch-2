pipeline {

  agent any

  stages {
    stage('build') {
      steps {
        bat 'javac -d . src/*.java'
        bat 'echo Main-Class: Rectangulator > MANIFEST.MF'
        bat 'jar -cvmf MANIFEST.MF rectangle.jar *.class'
      }
       post {
          success {
            archiveArtifacts artifacts: 'rectangle.jar', fingerprint: true
          }
        }
    }
    stage('run') {
      steps {
        bat 'java -jar rectangle.jar 7 9'
      }
    }
    stage('Promote Development to Master'){

          when {

           branch 'development'

          }

        steps {
           echo "Stashing Local Changes"
           bat "git stash"
           echo "Checking out Development"
           bat 'git checkout development'
           bat 'git pull origin development'
           echo 'Checking Out Master'
           bat 'git checkout master'
           echo "Merging Development into Master"
           bat 'git merge master'
           echo "Git push to origin"
           bat "git push origin master"
         }

       }
    }
}
