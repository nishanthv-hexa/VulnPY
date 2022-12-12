pipeline {
    agent any
  environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = credentials('secret_key')
        SEMGREP_PR_ID = "${env.CHANGE_ID}"

      //  SEMGREP_TIMEOUT = "300"
    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/nishanthv-hexa/VulnPY.git'
            }
            }
        
      stage('Semgrep-Scan') {
          steps {
            sh "pip3 install semgrep"
            sh "semgrep ci"
          }
      }
        stage ('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: '--format HTML --format XML ', odcInstallation: 'Owasp dependency Check'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
  stage('SonarQube Analysis') {
            steps {
                script{
    def scannerHome = tool 'Sonarscanner';
                withSonarQubeEnv('Sonarqube') {
      sh "${scannerHome}/bin/sonar-scanner"
    }
      }
            }
  }
        
        
//         stage('PMD'){
//             steps{
//             sh 'pmd -d -f txt -r rulesets/java/quickstart.xml -reportfile report.txt'}}
        stage ("bandit"){
           steps {
                sh 'bandit -r /var/lib/jenkins/workspace/SampleTest1 -f json -o /home/azureuser/archerysec-cli//banditResult.json'
            }}
      stage ("Archery Sync"){
        steps{
        sh 'sudo su'
        sh 'archerysec-cli -h http://localhost:8000 -t sUX5oFWhz5jYYkJ35-Ssv1872d7cd3qN7A2hpeRBVN7Wzayx05r9cG_5k2Lf8XaX --cicd_id=bc1b3b23-40ef-4e96-97e0-4a05fd0f5592 --project=b8296cd8-4717-4ee1-b178-248d5bcbfeb1 --bandit --report_path=$(pwd)'}
        }
}
}
