pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''# Create python virtual Environment
pip3 install virtualenv --user
python3 -m venv repo

. repo/bin/activate

# Install python requirements
pip install -r requirements.txt '''
      }
    }
    stage('Test') {
      steps {
        cobertura(coberturaReportFile: 'coverage.xml', failNoReports: true, failUnstable: true, failUnhealthy: true, lineCoverageTargets: '90,50,80')
        sh '''# Activate python virtual environment
. repo/bin/activate
#Run pytest and export coverage report
pytest --cov-report xml --cov-report term --cov ./lib/'''
      }
    }
  }
}