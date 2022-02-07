pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'pip3 install -r requirements.txt'
            }
        }
        state('Test') {
            steps {
                sh 'python3 manage.py test'
            }
        }
        stage('Deploy-to-prod') {
            steps {
                sh 'ssh -o StrictHostKeyChecking=no deploy.test@192.168.59.100 "source venv/bin/activate;\
                cd project; \
                git pull origin master; \
                pip install -r requirements.txt --no-warn-script-location; \
                python manage.py migrate; \
                deactivate; \
                sudo systemctl restart nginx; \
                sudo systemctl restart gunicorn"'
            }
        }

        stage('Deploy-to-prod') {
            input {
                message 'Shall we deploy to prod'
                ok 'Yes,please!'

            }
            steps {
                sh 'ssh -o StrictHostKeyChecking=no deploy2.test@192.168.59.101 "source venv/bin/activate;\
                cd project; \
                git pull origin master; \
                pip install -r requirements.txt --no-warn-script-location; \
                python manage.py migrate; \
                deactivate; \
                sudo systemctl restart nginx; \
                sudo systemctl restart gunicorn"'
            }
        }
    }
}