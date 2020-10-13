---
layout: post
title:  "Checking accessibility in Azure release pipelines"
date:   2020-10-13 11:58:25 +0100
categories: Azure Pipelines Accessibility
---

Given that all [UK public sector websites should now meet new accessibility rules](https://gcs.civilservice.gov.uk/blog/23-september-is-here-what-does-this-mean-for-you-and-accessibility/) I thought it would be useful to provide a simple example of running accessibility checks as part of an Azure release pipeline. In other words, every time there is a new deployment of a web application, the deployment can pass or fail based on it's accessibility (as well as any other functional and non functional tests you include).

Handily, there's an [extension to do just this](https://marketplace.visualstudio.com/items?itemName=DrewLewis.Accessibility) in the marketplace that supports WCAG Level A, WCAG Level AA, Section 508 and other best practices.

Once you've installed the extension you can add a task like this into your pipeline yaml (or add the same task in the classic editor):

```yaml
- task: AccessibilityChecker@0
  inputs:
    url: 'https://thewebappurltocheck'
    tagoptions: 'wcag2a'
```
This will then run the selected rules and the deployment will fail if there are any accessibility issues. in this case the deployment to the dev environment has failed as there is no alt tag in the specified image: 

![error as no alt tag on an image]({{ site.url }}/assets/accessibility_error.png)

You can look at the detailed output in the logs to see that there are also some minor violations of the standards that it would be good to fix as well:

![accessibility task detailed logs]({{ site.url }}/assets/accessibility_log.png)

If you want to try this then I've put an [example in GitHub](https://github.com/gidavies/BasicWebApp) with the instructions in the readme. This will build and deploy a web application (it happens to be .NET Core but could be anything) to Azure App Service (free tier), but will fail the accessibility checks as shown above. Note that there is no need for the web application to have been built and/or deploued in Azure pipelines, the accessibility task can be given any URL to check, however it was deployed.