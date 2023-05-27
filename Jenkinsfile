 pipeline{
    agent any
    parameters{
        string(name: 'flaskbucketname', defaultValue: 'flaskbucketname', description: 'bucket name')
        string(name: 'backendhost', defaultValue: 'backendhost', description: 'host name')
        string(name: 'frontendhost', defaultValue: 'frontendhost', description: 'frontend name')
        string(name: 'reactjsbucketname', defaultValue: 'reactjsbucketname', description: 'reactjs name')
        string(name: 'albEndpoint', defaultValue: 'backend alb endpoint', description: 'alb endpoint')
    }
    stages{
        stage('Build backend') {
            steps {
                script{
                sh '''
                cd backend
                echo 'Building Flask application...'
                rm -fr *.zip
                zip -r flask-$BUILD_NUMBER.zip *
                aws s3 cp flask-$BUILD_NUMBER.zip s3://${flaskbucketname}/
                scp dependencies.sh root@${backendhost}:/root/
                rm -fr *
                echo 'Flask application built successfully!'
                '''
                }
            }
        }
        stage('Deploy backend ') {
            steps {
                script{
                sh '''
                echo 'Deploying Flask application...'
                ssh root@${backendhost} "rm -rf *"
                aws s3 cp s3://${flaskbucketname}/flask-$BUILD_NUMBER.zip .
                scp flask-$BUILD_NUMBER.zip root@${backendhost}:/root/
                ssh root@${backendhost} "unzip flask-$BUILD_NUMBER.zip"
                ssh root@${backendhost} "sudo rm -rf *.zip"
                rm -fr *.zip
                echo 'Flask application deployed successfully!'
                '''
                }
            }
        }
        stage('Installation backend dependencies'){
            steps{
                script{
                sh '''
                echo 'installing the dependencies...'
                ssh root@${backendhost} "sh dependencies.sh"
                echo 'installed successfully'
                '''
                }
            }
        }
        stage('Run backend app'){
            steps{
                script{
                sh '''
                echo 'running the flask application'
                ssh root@${backendhost} "nohup python3 app.py >> output.log 2>&1 &"
                echo 'completed successfully'
                '''
                }
            }
        }

        stage('confirmation'){
            steps{
                script{
                    sh '''
                    ssh root@${backendhost} "sudo netstat -anlp | grep '80'"
                    '''
                }
            }
        }
        stage('Update frontend URL') {
            steps {  
                sh 'echo "Updating frontend URL in App.js"'
                sh "sed -i 's|\"backend url\"|\"${albEndpoint}\"|' ${WORKSPACE}/frontend/src/App.js"
                sh 'echo "updated.."'  
            }
        }        
        
        stage('Build frontend'){
            steps{
                script{
                    sh '''
                    cd frontend
                    echo "Building reactjs application"
                    rm -fr *.zip
                    zip -r reactjs-$BUILD_NUMBER.zip *
                    aws s3 cp reactjs-$BUILD_NUMBER.zip s3://${reactjsbucketname}/
                    echo "Flask application built successfully!"
                    '''
                }
            }
        }
        stage('Deploy frontend'){
            steps{
                script{
                    sh '''
                    echo "Deploying reactjs application..."
                    ssh root@${frontendhost} "rm -rf *"
                    aws s3 cp s3://${reactjsbucketname}/reactjs-$BUILD_NUMBER.zip .
                    scp reactjs-$BUILD_NUMBER.zip root@${frontendhost}:/root/
                    ssh root@${frontendhost} "unzip reactjs-$BUILD_NUMBER.zip"
                    ssh root@${frontendhost} "sudo rm -rf *.zip"
                    rm -fr *.zip
                    echo "reactjs application deployed successfully!"
                    '''
                }
            }
        }
 
        stage('Installation frontend dependencies'){
            steps{
                script{
                    sh '''
                    echo "installing......"
                    ssh root@${frontendhost} "rm -rf node_modules"
                    ssh root@${frontendhost} "yum install nodejs -y && npm install"
                    echo "installed successfully"
                    '''
                }
            }
        }
        stage('Run frontend app'){
            steps{
                script{
                sh ''' 
                echo "running the reactjs application"
                ssh root@${frontendhost} "nohup npm start >> npm_output.log 2>&1 &"
                echo "completed successfully"                
                '''
                }
            }
        }
    }
}