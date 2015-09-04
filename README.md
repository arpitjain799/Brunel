# Brunel Visualization

The Brunel Visualization project defines a highly succinct and novel language that defines interactive data visualizations based on tabular data.  The language is well suited for both data scientists and more aggressive business users.  The system interprets the language and produces visualizations using the user's choice of existing lower-level visualization technologies typically used by application engineers such as RAVE or D3 .  It can operate stand-alone and integrated into iPython notebooks with further integrations as well as other low-level rendering support depending on the desires of the community.

For Jupyter notebook integrations, Brunel Visualization supplies a REST API interprets the Brunel Visualization language and generates the required Javascript to display the visualization in a web browser.  Currently the app server must be  [Apache TomEE w/JAX-RS](http://tomee.apache.org/apache-tomee.html) with the brunel `.war` file deployed.  Instructions for setting this up are below.  We are working on improving and generalizing this.

For direct Java integrations, see:  `D3Builder` for creating D3 output, `ControlWriter` for adding interactive controls and `WebDisplay` as an example using these.  `BrunelService` also contains a usage example.

## Sub-Projects and Folders
* `core` Primary support for Brunel Visualization
* `data` The Brunel Visualization data processing engine
* `etc`  Examples and supporting utilities
* `lib`  Dependent .jars
* `service`  A basic web service to provide Brunel Visualization output
* `python`  Integration of Brunel Visualization for Jupyter (IPython)
* `R` Integration of Brunel Visualization for Jupyter (R)

## Builds

### Build using Gradle

#### Dependencies
* Install [Apache TomEE w/JAX-RS](http://tomee.apache.org/apache-tomee.html)
* Install [Gradle](https://gradle.org/).  
* Install Python 3 and also install `nose` using `pip install nose` (used for unit tests during build)

#### Set local environment variables:

    BRUNEL_SERVER=http://localhost:8080/brunel-service
    TOMCAT_HOME=[root dir of TomEE]

#### For a complete build:

```gradle build```

This will build all Java (including the '.war' file), run all tests, create Javadocs, collect all required Javascript and place all of this into `/brunel/out` 

#### To start the Brunel server

Gradle can be used to deploy Brunel to TomEE and start the web server:

```gradle cargoRunLocal```

To confirm it is working, navigate to:

    http://localhost:8080/brunel-service/docs

#### Next:

* Explore the docs to learn more
* Run `VisualTests` located in `/etc` to view a few Brunel syntax test examples
* See the `/python` and `/R` folders for instructions on how to use Brunel with Jupyter (IPython)
* For Java integrations, explore the javadocs generated by the Gradle build in `/out`

#### Details regarding JS/CSS files and locations:

Brunel uses a mix of static, translated and generated Javascript.

* Non-translated JS/CSS for Brunel is in `/core`.  These are expected to be edited by developers. 
* The translated `BrunelData.js` is created during the build of the `data` project.  Do not edit this file.
* `VisualTests.java` (in `/etc`) will copy the JS/CSS upon execution so they are in the expected locations
* Builds for `/service` will copy the JS/CSS in the correct locations for the .war file.  Additionally, the task `copyWebFiles` copies the JS/CSS in the expected place for execution directly from the src.
* Builds for `/python`, will copy the JS/CSS into a folder that also contains JS that is specific for the Jupyter integration 



