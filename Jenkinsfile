pipeline {
    agent any

    tools {
        jdk 'java8'
        gradle 'Latest'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Siddeshwarsid00/KotlinDagger.git'
            }
        }

        stage('Test') {
            steps {
                sh 'gradle test'
            }
        }

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'gradle sonarqube'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true, credentialsId: 'sonar-1'
                }
            }
        }

        stage('Clean & Build') {
            steps {
                sh 'gradle clean assembleDebug'
            }
        }

        stage('Publish to appCenter') {
            environment {
                APPCENTER_API_TOKEN = credentials('appCenter_tocken')
            }
            steps {
                appCenter   apiToken: APPCENTER_API_TOKEN,
                            ownerName: 'siddeshwarsid00-gmail.com', /*user name*/
                            appName: 'Android-cicd1', /*name as given in the appcenter for the project*/
                            pathToApp: '**/*.apk',
                            distributionGroups: 'TestingGroup'
            }
        }
    }
}
