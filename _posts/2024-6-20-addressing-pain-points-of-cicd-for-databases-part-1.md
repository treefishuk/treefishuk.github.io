---
layout: post
title: Addressing pain points of CI/CD for Databases - Part 1
--- 

Getting a CI/CD pipeline in place for web apps in Azure devops is easy-peasy. Unfortunately web apps often rely on these pesky things called "databases"... which are far less easy to get integrated and have multiple approaches of how to integrate them.

I went through a bit of an ordeal setting up database deployments for the [churchee](https://churchee.com) solution I'm working on. If you're reading this hopefully the pain I experienced will save you some!

I'm splitting all the stuff I came across into two parts.

- **Part 1:** (this part) is going to cover the problems and solutions I came across when working on the build pipeline.
- **Part 2** is going to cover the problems and solutions I came across when working on the release pipeline.

## Chosen Approach

The options I considered were:

- EF Core Migrations
- Flyway Migrations
- Database Model (Database Project in Visual Studio)

I decided I wasn't a fan of EF Core migrations and I didn't like the idea of having to remember to fire-up an additional tool outside Visual Studio. So I settled on the "Database Model", created a Database project based of the Database EF core had generated for my web app. Checked it in. Bosh.

## Problem 1: So what about when things change?

Pretty obvious problem to begin with, in that as soon as a new property on an entity class or an new entity class is added, the database project no longer matches what EF Core expects. "Just remember to sync it"... didn't seem like a good plan, as frankly, I'm forgetful! 

### Solution

I decided the best thing to do, would be as part of the CI build to generate a database with EF Core and then compare that database with the database project to make sure everything is in sync and to fail it if EF Core expects columns, tables etc. not in the DB project.

## Problem 2: Where to put the EF Core Generated Database

I could have created a database in Azure for the compare, but spinning up databases in Azure I have found to be pretty slow, and even if it doesn't exist for long, while it does exist it cost pennies. 

### Solution

I discovered that the "ubuntu-latest" Azure agent has docker installed! So I figured I could just deploy a SQL docker image as part of the build and publish to that. Great!

Here's the YAML:
```
- script: |
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 --hostname sql1 \
      -d \
      mcr.microsoft.com/mssql/server:2022-latest
```

## Problem 3: The ubuntu-latest agent can't build SQL Database projects (.sqlproj)

The tool I wanted to use to compare the DB Project and the EF Generated DB was the [Deploy Report](https://learn.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage-deploy-drift-report?view=sql-server-ver16) task that can be executed using SqlPackage. The "ubuntu-latest" agent though cant build can't build SQL Database projects (.sqlproj).

### Solution

Eventually I came accross [this article](https://jmezach.github.io/post/introducing-msbuild-sdk-sqlproj). Which mentions creating a second project which an SDK type of "MSBuild.Sdk.SqlProj" which references the .sql files in the database project to build the .dacpac. So the project looks something like this:

```
<Project Sdk="MSBuild.Sdk.SqlProj/2.7.2">
   <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <ImplicitUsings>enable</ImplicitUsings>
      <Nullable>enable</Nullable>
   </PropertyGroup>
   <ItemGroup>
      <Content Include="..\MyDatabaseProject.Database\dbo\**\*.sql" />
   </ItemGroup>
</Project>
```

And yep. Worked a treat! The .dacpac is successfully built using the "ubuntu-latest" agent.

## Problem 4: No SqlPackage.exe

Its hard enough trying to work out the location for SqlPackage on a windows build agent. On "ubuntu-latest" agent it's not there at all!.

### Solution

Installing SQLPackage on the "ubuntu-latest" agent using Powershell was super easy:

```
sudo dotnet tool install -g microsoft.sqlpackage
```

## Problem 5: Easily knowing what went wrong

So with the EF Core generated database being published to a SQL Docker container, I could use the Powershell installed SQLPackage to generate a deploy report using the .dacpac created with the project referencing the database project. But how do I get that from a SQLPackage Deploy report into a format that's useful, and can pass or fail the build if discrepancies are found?

### Solution

I decided to create an XSLT to transform the SQLPackage deploy report into a JUnit xml format. Turns out if you need to do something like this Chat GPT is really useful. Here's that code that Chat GPT helped me to put together:

```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns:dac="http://schemas.microsoft.com/sqlserver/dac/DeployReport/2012/02"
    exclude-result-prefixes="dac">

	<!-- Define the output format -->
	<xsl:output method="xml" indent="yes" encoding="UTF-8"/>

	<!-- Match the root element of the source XML -->
	<xsl:template match="/">
		<testsuites time="0">
			<testsuite name="DeploymentReport" time="0">
				<xsl:apply-templates select="//dac:Operation[@Name='Alter']"/>
				<xsl:apply-templates select="//dac:Operation[@Name='Create']"/>
				<xsl:apply-templates select="//dac:Operation[@Name='Drop']"/>
			</testsuite>
		</testsuites>
	</xsl:template>

	<xsl:template match="dac:Operation">
		<xsl:for-each select="dac:Item">
			<testcase>
				<xsl:attribute name="name">
					<xsl:value-of select="../@Name"/>
					<xsl:number value="position()" format="1" />
				</xsl:attribute>
				<xsl:attribute name="classname">
					<xsl:text>DeploymentReport.</xsl:text>
					<xsl:value-of select="../@Name"/>
					<xsl:number value="position()" format="1" />
				</xsl:attribute>
				<xsl:attribute name="time">0</xsl:attribute>

				<failure>
					<xsl:attribute name="type">
						<xsl:value-of select="../@Name"/>
					</xsl:attribute>
					<xsl:attribute name="message">
						<xsl:value-of select="@Value"/>
					</xsl:attribute>
				</failure>
			</testcase>
		</xsl:for-each>
	</xsl:template>
</xsl:stylesheet>
```

## More Problems and Solutions in Part 2

So at this point the build pipeline is generating a .dacpac. If the Database project goes out of sync with EF Core, the build fails and the "tests" tab on the build shows each of the discrepancies found. After all this the database still hasn't been deployed though, so check out part 2 for more problems, pain and work arounds! To the release pipeline!
