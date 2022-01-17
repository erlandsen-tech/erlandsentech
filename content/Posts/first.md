title: Welcome
date:  Sun Jan 16 18:50:25 CET 2022

----

## Humble beginnings

I built this site with a template engine for static sites and Azure Static Web App.
The build pipeline for the infrastructure is made using terraform cloud. This gives
me easily accessible CI/CD capabilities. For the web app, github actions is used.
The source code for the infrastructure can be found [here](https://github.com/varleg/erlandsentech_infrastructure)
and for the web app, [here](https://github.com/varleg/erlandsentech)

I will at some point do a complete writeup on the build process in the github action, as well
as how to set this up in azure static web apps. It was much easier than I first anticipated,
but my last encounter with azure webApps was a couple years ago when you even had to *buy*
the certificate, or go through a lengthy setup process with letsEncrypt. Now, even that, is done
for us. So this gives us a really cheap (free) website, with minimal work.

- [Pelican](https://blog.getpelican.com/)
- [Azure Static WebApps](https://azure.microsoft.com/en-us/services/app-service/static/)
