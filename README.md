# az-html-app (tw2k-html-app in azure)
A hello world app for using ARM templates of azure for testing Infrastructure as code in Azure.

https://tw2k-html-app.azurewebsites.net/

- TODO: CD

## Create/Update infrastructure
https://docs.microsoft.com/en-us/azure/app-service/app-service-web-get-started-html


### Create the target resource group
```
az group create \
  --name rg-az-html-app \
  --location westus
```

### Create/update resources -- deploy ARM template
```
az group deployment create \
  --name initial-resources \
  --resource-group rg-az-html-app \
  --template-file azuredeploy.json \
  --parameters azuredeploy.parameters.json
```


### Deploy build
```
npm run build
pushd ./build
zip -r ../built-html.zip .
popd
az webapp deployment source config-zip \
  --resource-group rg-az-html-app \
  --name tw2k-html-app  \
  --src built-html.zip
```

Zip deploy:
https://docs.microsoft.com/en-us/azure/app-service/deploy-zip#deploy-zip-file-with-azure-cli
```
az webapp deployment source config-zip  -g <resource-group>  -n <appname>  --src <filepath>.zip
```

Example output (wondering about the credentials aspect)
```
╰─➤  az webapp deployment source config-zip \                                                                                                                               130 ↵
  --resource-group rg-az-html-app \
  --name tw2k-html-app  \
  --src built-html.zip
Getting scm site credentials for zip deployment
Starting zip deployment. This operation can take a while to complete ...
Deployment endpoint responded with status code 202
{
  "active": true,
  "author": "N/A",
  "author_email": "N/A",
  "complete": true,
  "deployer": "Push-Deployer",
  "end_time": "2020-02-09T23:26:34.0326219Z",
  "id": "1b6a8b029b334dc381ca08168c830574",
  "is_readonly": true,
  "is_temp": false,
  "last_success_end_time": "2020-02-09T23:26:34.0326219Z",
  "log_url": "https://tw2k-html-app.scm.azurewebsites.net/api/deployments/latest/log",
  "message": "Created via a push deployment",
  "progress": "",
  "received_time": "2020-02-09T23:26:30.8918557Z",
  "site_name": "tw2k-html-app",
  "start_time": "2020-02-09T23:26:31.0400113Z",
  "status": 4,
  "status_text": "",
  "url": "https://tw2k-html-app.scm.azurewebsites.net/api/deployments/latest"
}
```

### Destroy
```
az group delete --name "rg-az-html-app"
```

### Creating jenkins service principle

╰─➤  az ad sp create-for-rbac --name local-jenkins-service-principal
Changing "local-jenkins-service-principal" to a valid URI of "http://local-jenkins-service-principal", which is the required format used for service principal names
Creating a role assignment under the scope of "/subscriptions/a67f9a16-b22e-48dc-8516-015e88ef709e"
  Retrying role assignment creation: 1/36
  Retrying role assignment creation: 2/36
{
  "appId": "70d862c5-e74d-4162-8df3-d66d6eb2fbc6",
  "displayName": "local-jenkins-service-principal",
  "name": "http://local-jenkins-service-principal",
  "password": "REDACTED",
  "tenant": "ffe8f317-d65f-4b00-ad3b-4982fc85ae02"
}

az role assignment list --assignee APP_ID
az role assignment create --assignee APP_ID --role Contributor

az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID

az login --service-principal --username 70d862c5-e74d-4162-8df3-d66d6eb2fbc6 --password 61ddaefd-a86b-4315-90e2-c2a3b663a4ce --tenant ffe8f317-d65f-4b00-ad3b-4982fc85ae02
