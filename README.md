jca-quickstart
==============

Get started with the Java EE Connector Architecture

Our goal is to provide an entry point to successful development of JCA connectors. This very powerful part of the JavaEE spec is currently underestimated, as getting started with this technology has been quite difficult so far. In some forums it is even referred to as some sort of black art, which is fortunately only true if you try to get something done with JCA for the first time.
In order to demystify this technology we started this collection of explanations, examples, links and available connectors, hoping that it will help some of you to harness the possibilities of JCA.

# Foreword
A few thoughts worth mentioning before we get started:

* If you ended up here you are most probably on the right track.
* JCA does appear quite difficult in the beginning, although it is no kind of rocket science. Keep on going…
* If you want things to change, you can always make suggestions for improvement. In fact this is highly appreciated. There is one good proposal about how to change inbound connectors already [here](https://github.com/dblevins/mdb-improvements).

# General explanations
JCA supports two ways of communication:

* inbound
* outbound

While inbound connectors are relatively simple to setup, outbound connectors require an additional step to configure the connection pool. Still, both are quite ok to use once you got the hang of how it works.




# Java EE Compatible Connectors

* http://xadisk.java.net/ (A connector providing transactional file I/O)
* https://github.com/ljnelson/drools-jca 
* http://genericjmsra.java.net/ (A connector to integrate JMS messaging into Java EE application servers)

## Links

* https://github.com/dblevins/mdb-improvements (A proposal on how to improve writing inbound JCA connectors)
* http://blog.dblevins.com/2010/10/ejbnext-connectorbean-api-jax-rs-and.html (A good lookout on what could be done with an improved JCA API)
* http://jcp.org/en/jsr/detail?id=322 (The JCA specification)


# Examples

## Adam Bien's connectorz
[Adam Bien's connectorz](http://connectorz.adam-bien.com/) on adam-bien.com.

### What it does

The sample project provides two connectors out-of-the-box:

* Work Manager: JCA implementation of java.util.concurrent.Executor
* File Store: JCA implementation of transactional file access.

The setup description below explains the setup of the file store conector.

### How to set it up...

#### …on Glassfish

Prerequisites:

* Working maven installation (with the %PATH% variable pointing to your installation)
* Glassfish (3.1.2 or later) installed and running (make sure the asadmin command is in your %PATH%)
* [Github repo](https://github.com/AdamBien/connectorz) cloned to local drive

#### … using the command line

Then perform the following steps to setup the files connector:

go to checked out project dir

  cd $your_workspace$/connectorz/files

then do
	
	mvn install

wait until it's done, then do	

	cd store/target
	asadmin deploy --name filesConnector jca-file-store.rar
	asadmin create-connector-connection-pool --raname filesConnector --connectiondefinition org.connectorz.files.BucketStore files/pool
	asadmin create-connector-resource --poolname files/pool jca/files

	cd ../..
	cd client/target
	asadmin deploy jca-file-client.war
	
#### … using Netbeans

todo

#### … using IntelliJ IDEA

Use the command line, as IDEA currently does not support Resource Adapter Archives (.rar) with its own artifacts. Therefore you'd have to package the connector externally using maven anyway.

#### … using Eclipse

todo (volunteers wanted :-)

#### Test it
	
Afterwards you can use your favorite REST client to test the application Adam provided with his example.
As a REST client you can use either [curl](http://curl.haxx.se/) or [rest-client](http://code.google.com/p/rest-client/); or any other client of your choice.
The URI for creating a file is:

	http://localhost:8080/jca-file-client/v1/files/file_to_create
	
Be aware that you need to do a PUT request to store files and a GET with the same URI to retrieve a stored file.

	