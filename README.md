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