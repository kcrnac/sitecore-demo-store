# Run Sitecore demo store example

## Prerequisites

Ensure you have followed the steps listed on the [installation documentation](https://sitecore.github.io/Helix.Examples/install.html).

The Helix examples assume you have some experience with (or at least an understanding of) Docker container-based Sitecore development. For more information, see the [Sitecore Containers Documentation](https://containers.doc.sitecore.com).

### .NET Core 3.1

Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) if you do not have it.

## Initialize

Open a PowerShell administrator prompt and run the following command, replacing the `-LicenseXmlPath` with the location of your Sitecore license file.

```
.\init.ps1 -LicenseXmlPath C:\License\license.xml
```

You can also set the Sitecore admin password using the `-SitecoreAdminPassword` parameter (default is "b").

This will perform any necessary preparation steps, such as populating the Docker Compose environment (.env) file, configuring certificates, and adding hosts file entries.

## Build the solution and start Sitecore

Next run the following command.

```
docker-compose up -d
```

This will download any required Docker images, build the solution and Sitecore runtime images, and then start the containers. The example uses the *Sitecore Experience Management (XM1)* topology.

Once complete, you can access the instance with the following.

* Sitecore Content Management: https://cm.sitecore-demo-store.localhost
* Sitecore Identity Server: https://id.sitecore-demo-store.localhost
* Basic Company site: https://www.sitecore-demo-store.localhost

## Push and publish items

First, restore the `sitecore` CLI dotnet tool, which will be used for Sitecore login and serialization.

```
dotnet tool restore
```

Login the `sitecore` CLI with the following command.

```
dotnet sitecore login --cm https://cm.sitecore-demo-store.localhost/ --auth https://id.sitecore-demo-store.localhost/ --allow-write true
```

This will open a browser tab with the Sitecore login page. Use "admin" and the password you specified on init ("b" by default).

Once logged in, push the latest serialized items using the `sitecore` CLI by running the following command.

```
dotnet sitecore ser push
```

Finally, publish using the `sitecore` CLI.

```
dotnet sitecore publish
```

You should now be able to view the Basic Company site at https://www.sitecore-demo-store.localhost.

## Stop Sitecore

When you're done, stop and remove the containers using the following command.

```
docker-compose down
```

## Credits

This project has been inspired by [Sitecor helix examples](https://github.com/Sitecore/Helix.Examples).
