
node {
    withEnv([
        'AZURE_SUBSCRIPTION_ID=518972eb-adf9-49c6-b5eb-2ae837b9efef',
        'AZURE_TENANT_ID=0d533360-8780-4196-86c8-3b16d02ecf3b'
    ]) {
        stage('init') {
            checkout scm
        }

        stage('build') {
            sh 'mvn clean package'
        }

        stage('deploy') {
            withCredentials([usernamePassword(
                credentialsId: 'AzureServicePrincipal',
                usernameVariable: 'AZURE_CLIENT_ID',
                passwordVariable: 'AZURE_CLIENT_SECRET'
            )]) {
                sh '''
                    az login --service-principal \
                        --username "$AZURE_CLIENT_ID" \
                        --password "$AZURE_CLIENT_SECRET" \
                        --tenant "$AZURE_TENANT_ID" \
                        --output none

                    az account set --subscription "$AZURE_SUBSCRIPTION_ID"

                    az webapp deploy \
                        --resource-group jenkins-get-started-rg-sweden \
                        --name siddharth-jenkins-calculator \
                        --src-path target/calculator-1.0.war \
                        --type war \
                        --target-path /home/site/wwwroot/webapps/ROOT.war
                '''
            }
        }
    }
}
