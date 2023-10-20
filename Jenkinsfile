pipeline {
agent any
stages {
stage ('Build') {
steps {
sh '''#!/bin/bash
python3.7 -m venv test
source test/bin/activate
pip install pip --upgrade
pip install -r requirements.txt
'''
}
}
stage ('test') {
steps {
sh '''#!/bin/bash
source test/bin/activate
pip install pytest
py.test --verbose --junit-xml test-reports/results.xml
'''
}
post{
always {
junit 'test-reports/results.xml'
}
}
}
stage ('Clean') {
agent {label 'awsDeploy || awsDeploy2'}
steps {
sh '''#!/bin/bash
if pgrep -f "gunicorn" > /dev/null; then
  # If it's running, kill all gunicorn processes
  pkill -f "gunicorn"
  echo "Stopped all gunicorn processes."
else
  echo "No gunicorn processes found."
fi
exit 0
fi
'''
}
}

stage ('Reminder') {
steps {
sh '''#!/bin/bash
##############################################################
# The Application should be running on your other instance!! #
##############################################################
'''
}
}
}
}
