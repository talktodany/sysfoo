pipeline {
  agent none

  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'compiling maven app'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'test maven app'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
       when {
             beforeAgent true
             branch  'master'
           }

      steps {
        echo 'Package maven Step'
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.war'
      }
    }

     stage('Deploy to Dev') {
       when {
             beforeAgent true
             branch  'master'
           }

      agent any

      steps {
        echo 'Deploying to Dev Environment with Docker Compose'
        sh 'docker-compose up -d'
      }
    }

    stage('Docker BuildandPublish') {
      
      when {
             beforeAgent true
             branch  'master'
           }

      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("talktodany/sysfoo:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
            dockerImage.push("dev")
          }
        }

      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}
