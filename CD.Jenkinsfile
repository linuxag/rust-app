pipeline
{
    agent
    {
        label 'worker1'
    }
    stages
    {
        stage('send-notification')
        {
            
            steps{
                /*script 
                {
                    emailext subject: '${JOB_NAME} - ${BUILD_NUMBER} ', body: 'Job url : ${BUILD_URL}',  to: 'jsnrahul@gmail.com'
                }*/
                sh '''
                echo "notification sent"
                '''
            }
        
        }
        stage('static-analysis-k8deploy')
        {
            steps{
            sh '''
            #static analyisi of Deployment
            #docker run -t -v $(pwd):/output bridgecrew/checkov -f /output/rust-cart-app1-deployment.yml -o json |  jq '.' > k8deploy_result | exit 0            
            #cat k8deploy_result  | grep -B 2 FAILED
            echo "sacnning dockerfile"
            '''
            }
        }
        stage('deploy')
        {
            
            steps{
                sh '''
                sed -i 's/unique_tag/latest/g' rust-cart-app1-deployment.yml
                #echo "kubectl deploy"
                kubectl apply -f rust-cart-app1-deployment.yml
                '''
            }
        
        }
        stage('smoke-test')
        {
            
            steps{
                sh '''
                sleep 5
                curl 192.168.0.2:30022
                '''
            }
        
        }
        
    }
}