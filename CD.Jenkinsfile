pipeline
{
    agent
    {
        label 'worker1'
    }
    parameters { 
    string(name: 'app_version', defaultValue: 'v1', description: 'app_version') 
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
                pwd
                ls -ltr
                sed -i "s/unique_tag/$app_version/g" rust-cart-app1-deployment.yml
                
                #cat rust-cart-app1-deployment.yml | grep app_version

                #echo "kubectl deploy"
                kubectl delete -f rust-cart-app1-deployment.yml | exit 0
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