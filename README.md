# Adventure Builder

To run tests execute: mvn clean install

To launch a server execute in the module's top directory: mvn clean spring-boot:run

To launch all servers execute in bin directory: startservers

To stop all servers execute: bin/shutdownservers

To run jmeter (nogui) execute in project's top directory: mvn -Pjmeter verify. Results are in target/jmeter/results/, open the .jtl file in jmeter, by associating the appropriate listeners to WorkBench and opening the results file in listener context.

To check if what process is running on a given port:

`fuser <number_of_port>/tcp`

To kill that same process:

`fuser -k <number_of_port>/tcp`

Test values for forms:

`curl -v -d "var1=value1&var2=value2..." -X POST <path_where_to_post>`
