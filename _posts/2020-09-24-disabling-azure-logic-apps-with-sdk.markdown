---
layout: post
title:  "Disabling Azure Logic Apps with the C# SDK"
date:   2020-09-24 11:58:25 +0100
categories: Azure LogicApps SDK
---

I was asked how to disable and enable Azure Logic Apps from C# but my search skills let me down in finding an example. That left me with my standard option of finding something similar and hacking it about. To that end I found a [good blog post on using the .NET SDK](https://www.serverless360.com/blog/managing-azure-logic-apps-using-net-sdk) that walks through a more comprehensive provisioning of a Logic App. I've adapted and simplified the code to focus only on disabling and enabling Azure Logic Apps. 

My (definitely not production) .NET Core console app code can be found here [https://github.com/gidavies/LogicAppManager](https://github.com/gidavies/LogicAppManager).

Most of the code covers authentication into Azure and the code to actually disable and enable Logic Apps is extremely simple:

```
if (logicAppState)
  logicManagementClient.Workflows.Enable(logicAppResourceGroupName, logicAppName);
else
  logicManagementClient.Workflows.Disable(logicAppResourceGroupName, logicAppName);
```

I’ve put the instructions in the ReadMe, you do need to create an Azure service principal and then add the ids and secrets into the arguments to the app, but the original linked blog post in the ReadMe walks through that.