pipeline{
    agent any
    environment { 
        VIRTUAL_ENV = 'venv' 
    }

    stages
    { 
        stage('Setup') {
            steps{
                script {
                    // Check for virtual environment
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) { 
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    // Install requirements, including the new tools
                    bat "%VIRTUAL_ENV%\\Scripts\\activate.bat && pip install -r requirements.txt && pip install coverage bandit"
                }
            }
        }
        
        stage('Lint'){
            steps {
                script {
                    bat "%VIRTUAL_ENV%\\Scripts\\activate.bat && flake8 app.py"
                }
            }
        }
        
        stage('Test') {
            steps{
                script {
                    // Run pytest using the 'coverage' tool
                    bat "%VIRTUAL_ENV%\\Scripts\\activate.bat && coverage run -m pytest"
                }
            }
        }
        
        stage('Coverage') {
            steps{
                script {
                    // Generate the coverage report in the console
                    bat "%VIRTUAL_ENV%\\Scripts\\activate.bat && coverage report"
                }
            }
        }
        
        stage('Security Scan') {
            steps{
                script {
                    // Run bandit to scan app.py. We add '-r' to recursively find it.
                    bat "%VIRTUAL_ENV%\\Scripts\\activate.bat && bandit -r app.py"
                }
            }
        }

        stage('Deploy'){
            steps {
                script {
                    echo "Deploying application..."
                    // A simple local 'deployment' step 
                    bat "%VIRTUAL_ENV%\\Scripts\\activate.bat && python app.py"
                }
            }
        }
    }
    post {
        always{
            cleanWs()
        }
    }
}