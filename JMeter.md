# JMeter Maven Plugin
(based on the following [blogpost](https://www.blazemeter.com/blog/how-use-jmeter-maven-plugin))

If you are using Maven and you need to load test your projects, you can use the [JMeter Maven plugin](https://github.com/jmeter-maven-plugin/jmeter-maven-plugin). This plugin enables you to run tests from within the Maven project, instead of running performance tests as scripts in [JMeter](http://jmeter.apache.org/). This blog post will go over how to run your [JMeter](http://jmeter.apache.org/) test from Maven, and how to view the results.


## Running a JMeter Test with the JMeter Maven Plugin

Let’s take a JMeter script and see how you can run it from Maven as part of the build. The JMeter Maven plugin allows you to do just that.

1. First of all, you need to add your plugin to the `pom.xml` of your project.

Here you must add the plugin. You can find the basic configuration [here](https://github.com/jmeter-maven-plugin/jmeter-maven-plugin/wiki/Basic-Configuration). You just need to copy the configuration text and paste it in your `pom.xml` file.

Finally, you have a `pom.xml` file that looks like this:

```
<plugins>
  <!-- JMeter -->
  <plugin>
    <groupId>com.lazerycode.jmeter</groupId>
    <artifactId>jmeter-maven-plugin</artifactId>
    <version>${version.com.lazerycode.jmeter}</version>
    <inherited>false</inherited>
    <executions>
      <execution>
        <id>jmeter-tests</id>
        <phase>verify</phase>
        <goals>
          <goal>jmeter</goal>
        </goals>
      </execution>
    </executions>
    <configuration>
      <jmeterVersion>4.0</jmeterVersion>
      <testFilesDirectory>jmeter/tests/</testFilesDirectory>
    </configuration>
  </plugin>
</plugins>
```

2. Create a `jmeter/tests` directory, and place your JMeter load test there. You can also create `jmeter/data` and add your data in csv notation there.

When running the project, the JMeter Maven plugin searches for tests to run in this directory. Have a look at the `jmeter` folder and inspect the files there.

3. No you can execute your plugin by using Maven. Go to your project directory and run the following command in the command line:

```
$ mvn -Pjmeter verify
```

If the test ran successfully, the raw results are located at /target/jmeter/results. You will find a report csv-file named `20180425-Adventure.csv`:

```
timeStamp,elapsed,label,responseCode,responseMessage,threadName,dataType,success,failureMessage,bytes,sentBytes,grpThreads,allThreads,Latency,IdleTime,Connect
1524681145581,3926,Create Broker,200,,setUp Thread Group 1-1,text,true,,1640,356,1,1,3202,0,189
1524681149511,118,Create Adventures,200,,setUp Thread Group 1-1,text,true,,2649,417,1,1,84,0,0
1524681149629,64,Create Adventures,200,,setUp Thread Group 1-1,text,true,,3102,417,1,1,37,0,0
1524681149693,62,Create Adventures,200,,setUp Thread Group 1-1,text,true,,3555,417,1,1,32,0,0
...
...
```

##### CSV Log format

The CSV log format depends on which data items are selected in the configuration. Only the specified data items are recorded in the file. The order of appearance of columns is fixed, and is as follows:

- timeStamp - in milliseconds since 1/1/1970
- elapsed - in milliseconds
- label - sampler label
- responseCode - e.g. 200, 404
- responseMessage - e.g. OK
- threadName
- dataType - e.g. text
- success - true or false
- failureMessage - if any
- bytes - number of bytes in the sample
- sentBytes - number of bytes sent for the sample
- grpThreads - number of active threads in this thread  group
- allThreads - total number of active threads in all groups
- URL
- Filename - if Save Response to File was used
- latency - time to first response
- connect - time to establish connection
- encoding
- SampleCount - number of samples (1, unless multiple samples are aggregated)
- ErrorCount - number of errors (0 or 1, unless multiple samples are aggregated)
- Hostname - where the sample was generated
- IdleTime - number of milliseconds of 'Idle' time (normally 0)
- Variables, if specified

More information on this topic is [here](https://jmeter.apache.org/usermanual/listeners.html). Note that you can configure JMeter to output the results in other formats other than csv (e.g., XML).

## Reports

Since version [`2.2.0`](https://github.com/jmeter-maven-plugin/jmeter-maven-plugin/blob/master/CHANGELOG.md), html generation is built-in:

Just add this in configuration element:

```
<generateReports>true</generateReports>
```

Just try this out, and check the output in the results folder.
