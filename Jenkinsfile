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
        sh '''# Activate python virtual environment
. repo/bin/activate
#Run pytest and export coverage report
pytest --cov-report xml --cov-report term --cov ./lib/'''
        cobertura(coberturaReportFile: 'coverage.xml', failNoReports: true, failUnstable: true, failUnhealthy: true, lineCoverageTargets: '90,50,80')
      }
    }
    stage('deployment') {
      when {
        branch 'master'
      }
      steps {
        waitUntil() {
          input 'can we deploy?'
          sh '''cd deployment
sh provision.sh'''
        }

      }
    }
  }
}