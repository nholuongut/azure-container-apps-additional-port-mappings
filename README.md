### Test app for Azure Container Apps additional port mappings

![](https://i.imgur.com/waxVImv.png)
### [View all Roadmaps](https://github.com/nholuongut/all-roadmaps) &nbsp;&middot;&nbsp; [Best Practices](https://github.com/nholuongut/all-roadmaps/blob/main/public/best-practices/) &nbsp;&middot;&nbsp; [Questions](https://www.linkedin.com/in/nholuong/)
<br/>

#### Deploy the apps

```bash
# deploy an app that listens to multiple ports
az containerapp up -n multi-port-app -g $RESOURCE_GROUP --environment $ENVIRONMENT \
    --env-vars 'PORTS=3000,3001,3002,3003' --source .

# deploy an app to test the other app
az containerapp up -n multi-port-tester -g $RESOURCE_GROUP --environment $ENVIRONMENT --source .

# update the first app to add port mappings
# (need to use REST API for now because az containerapp update is still using older API version)

# get the id of the first app
MULTI_PORT_APP_ID=`az containerapp show -n multi-port-app -g $RESOURCE_GROUP --query id -o tsv`
# update the app
az rest -m PATCH -u "$MULTI_PORT_APP_ID?api-version=2023-05-02-preview" --body @multi-port-app.json
```

#### Test it

```bash
# if this doesn't work, the tester app might be scaled down, 
# hit its public endpoint to scale it up
az containerapp exec -n multi-port-tester -g $RESOURCE_GROUP

# once inside the container, use curl to test the other app
## this hits the main port
curl http://multi-port-app

## this hits the additional ports
curl http://multi-port-app:3001
curl http://multi-port-app:3002
curl http://multi-port-app:8003

## these shouldn't work
curl http://multi-port-app:3000
curl http://multi-port-app:3003
```

You can also use the browser to test the apps.

This example will use the tester to call the first app and return the response:
`https://multi-port-tester.<region>.azurecontainer.io/curl/http://multi-port-app:3001`

# 🚀 I'm are always open to your feedback.  Please contact as bellow information:
### [Contact ]
* [Name: nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com)

![](https://i.imgur.com/waxVImv.png)
![](Donate.png)
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.🌟