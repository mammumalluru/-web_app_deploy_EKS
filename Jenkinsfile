pipeline{

    agent any



    stages{

        stage('git checkout'){

            steps{

                echo "Pulling latest code..."
                    git branch: 'main',
                    url: 'https://github.com/mammumalluru/-web_app_deploy_EKS.git', credentialsId: 'git-creds'
            }
        }

        stage('build Docker Image'){

            steps{

                
            }
        }


        
    }
}