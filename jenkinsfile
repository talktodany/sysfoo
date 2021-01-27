pipeline {
  agent any
  
tools{
  maven 'Maven 3.6.3'
}

  stages{
      stage("build"){
          steps{
              echo 'compiling maven app'
              sh 'mvn compile'
          }
      }
      stage("test"){
          steps{
              echo 'test maven app'
              sh 'mvn clean test'
          }
      }
      stage("package"){
          steps{
              echo 'Package maven Step'
              sh 'mvn package -DskipTests'
          }
      }
  }

  post{
    always{
        echo 'This pipeline is completed..'
    }
  }
}