pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'feat-labdevsecops', url: 'https://github.com/Taller-DevSecOps/devsecops-laboratorio.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('miSonar') {
                    // Ajusta prámetros de tu proyecto
                    bat '''
                        sonar-scanner \
                          -Dsonar.projectKey=labsonar-cursoseg \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=$SONAR_HOST_URL \
                          -Dsonar.login=$sonar-token
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Espera el resultado del análisis y falla si no pasa el gate
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo '✅ Análisis SonarQube aprobado. No se detectaron vulnerabilidades críticas.'
        }
        failure {
            echo '❌ Quality Gate falló. Revisa vulnerabilidades y problemas en SonarQube.'
        }
    }
}
