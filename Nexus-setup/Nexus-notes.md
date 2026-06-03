SonaType Nexus
================

=> Nexus is free and Open source software (OSS)

=> It is used as Artifactory Server

=> It is used to store project build artifacts

		Ex: jar, war etc...

=> Project artifacts will be stored in artifactory server for backup purpose.

=> Nexus s/w developed using java language.

=> The current version of nexus is 3.x

Note: The alternate for nexus tool is jfrog.

============================================================
Q) What is difference between Nexus repo and GitHub repo ?
============================================================

-> Github is a SCM software which is used to store source code of the project.

-> Nexus is an Artifactory Repository server which is used to store project build artifacts (jar/war).

=============
Nexus Setup
=============

Git Repo : https://github.com/kumarmoger/Sonar-Nexus/blob/main/Nexus-setup/Nexus-setup-Docker.md

================================
Working with Nexus Repository
================================

=> We will create a Repository to store project artifacts

=> We can use 2 types of repositories in nexus

		1) snapshot repository

		2) release repository

-> If project is under development then that project build artifacts will be stored into snapshot repository.

-> If project development already completed and released to production then that project build artifacts will be stored to release repository.

Note: Based on <version/> name available in project pom.xml file it will decide artifacts should be stored into which repository.


<version>0.0.1-SNAPSHOT</version> => It will go to snapshot repository

<version>RELEASE-1.0</version> => It will go release repository


-> Create Repositories by selecting "Maven 2 (Hosted)"

		- select policy


Snapshot repo url : http://3.24.182.235:8081/repository/kumar-snapshot-repo/

Release repo url : http://3.24.182.235:8081/repository/kumar-release-repo/

==================================================
Integrate Nexus Server Repos in our Maven Project
==================================================

## Step-1:  Nexus Repos details we will configure in project pom.xml file like below


  <distributionManagement> 

		<repository>
			<id>nexus</id>
			<name>Kumar Releases Nexus Repo</name>
			<url>http://3.24.182.235:8081/repository/kumar-release-repo/</url>
		</repository>
		
		<snapshotRepository>
			<id>nexus</id>
			<name>Kumar Snapshots Nexus Repo</name>
			<url>http://3.24.182.235:8081/repository/kumar-snapshot-repo/ </url>
		</snapshotRepository>	

  </distributionManagement>


## Step-2: We need to configure Nexus Server Credentials in Maven "settings.xml" file  

		Maven settings.xml file Location : C:\apache-maven-3.8.5\conf


## Step-3:  Once these details are configured then we can run below maven goal to upload build artifacts to Nexus Server

	$ mvn clean deploy		