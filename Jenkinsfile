pipeline {
  agent any
  stages {
    stage('Checkout & Build') {
      steps {
        echo 'Pipeline kicked off'
        git 'https://github.com/SandeepMukala/Mule4CICDTest.git'
        bat 'mvn clean install package -DskipTests=true'
      }
    }
    stage('Munit Test') {
      steps {
        bat 'mvn test'
      }
    }
    stage('Deploy to cloud hub') {
      steps {
        script {
          def workerSizeInput = input(
            id: 'workerSizeInput', message: 'Please select vCores for Deployment:?',
            parameters: [
              [
                $class: 'ChoiceParameterDefinition', choices: '0.1\n0.2\n1\n2\n4\n8\n16',
                name: 'SELECTED_WORKER_VERSION',
                description: 'Please select worker version'
              ]])
              vCoreInput = "${workerSizeInput}"
              bat "echo $vCoreInput"
              bat 'echo %WORKSPACE%'
              bat 'echo deploying artifact to cloud hub'
              bat "anypoint-cli runtime-mgr cloudhub-application deploy --runtime 4.1.3 --workerSize $vCoreInput \"mule4-cicd\" \"%WORKSPACE%/target/mule4test-1.0.0-SNAPSHOT-mule-application.jar\" --username=\"sandeep_m\" --password=\"Whishworks@2018\""
            }

          }
        }
      }
    }