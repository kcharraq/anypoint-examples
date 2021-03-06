#Mule Expression Language Basics

This example application contains few simple flows that introduce most of the basic implementations of Mule Expression Language (MEL).

###Assumptions

This document assumes you are familiar with Mule Expression Language's basic syntax and are comfortable building and running Mule applications using Mule Studio or XML.

If you aren't yet familiar with how to access information about the Mule messages that pass through your applications, consider following this tutorial, which walks you through both examining your Mule message and its data structure and writing simple MEL expressions.

###Set up and run the example

1. Import the Mule example project into your workspace and run it as a mule application

2. ****Flow 1**** : Through a web browser, access the URL **http://localhost:8081/greet1?username=yourName** 
The response prints the words **Hello (yourName)** in your browser.

3. ****Flow 2**** : Through a web browser, access the URL **http://localhost:8081/greet2?username=yourName**. This prints the words **Hello (yourName)** in your browser.
Then, access the URL again, but this time do not include any parameters. Verify that the expected output is received.

4. ****Flow 3**** : Through a web browser, access the URL **http://localhost:8081/greet3?username=yourName&age=22**. This will print the words **Hello (yourName)** in your browser and also save a csv file that contains this data, plus the value true for the boolean parameter.

5. ****Flow 4**** : In a browser, access the URL **http://localhost:8081/greet4?username=yourName&age=22**. This will print the words **Hello (yourName)** in your browser and also save a csv file that contains this data, plus the value true for the boolean parameter.
 

6. ****Flow 5**** : You must now send the HTTP endpoint an HTTP request that includes a body with an attached XML file. Send a POST request to **http://localhost:8081/greet5** attaching an XML to the body of the message. A sample XML is provided below.

	The easiest way to do this is to send a POST via a browser extension such as Postman (for Google Chrome) or the curl command line utility.

       < user >
     
       < username >
       test
       < /username >
     
       < age > 21 < /age >
      
       < /user >
 
    This will print the words Hello yourName in your browser and also save a csv file that contains this data, plus the value true for the boolean parameter.


7. ****Flow 6**** : You must now send the HTTP endpoint an HTTP request that includes a body with an attached JSON file. Send a POST request to **http://localhost:8081/greet6**, attaching a JSON object the body of the message. A sample JSON is provided below.

   The easiest way to do this is by sending a POST via a browser extension such as Postman (for Google Chrome) or the curl command line utility.

       { "username": "test", "age" : 21 }
 
   This will print the words Hello yourName in your browser and also save a csv file that contains this data, plus the value true for the boolean parameter.
 


#####Flow 1 ??? Accessing Properties

This flow creates a simple web service that takes an HTTP request that includes a username parameter and returns a greeting using that username.

In this example, you use MEL to:

* access an inbound property
* dynamically set the payload
	
This method of accessing query string parameters does not escape URL encoded characters (such as spaces, which would be read as %20). If you want to access the escaped values of these parameters, you can do it through a special inbound property provided by Mule named http.query.params which contains a map, where keys are property names and values are escaped property values.

In this flow, you can get a user name with escaped characters through this expression:

Hello #[message.inboundProperties.'http.query.params'.username]
 
#####Flow 2 ??? Dynamic Routing by Evaluating a Condition

In the previous flow, if your call to the service doesn't include a username parameter, it results in an error. You can prevent this from happening by adding some flow control components. This example includes a Choice Router that verifies if the required parameter is being passed.

In this flow, you use MEL to:

* evaluate conditions in a choice component
* access an inbound property
* dynamically set the payload
 
#####Flow 3 ??? Variable Assignment and Evaluating Conditions

In this flow, the service saves a CSV file with user data besides just returning a greeting. The call to the service will now include two parameters, username and age. The service stores these two parameters and adds a third boolean parameter that evaluates if the user is underage (if age is > 18).

In this flow, you will use MEL to:

* set a flow variable in the message
* generate an output based on evaluating the input
* access an inbound property
* dynamically set the payload


#####Flow 4 ???  Creating Maps and Evaluating Conditions with DataMapper

In this flow, like in the previous one, the Mule application saves a CSV file with user data and returns a greeting. The call to the service includes two parameters, username and age. The service stores these two parameters and adds a third boolean parameter that evaluates if the user is above a certain age (if age > 18). In this case, however, the mapping between input and output fields and the generation of the third field is performed by a DataMapper component.

In this example, you will use MEL to:

* set a flow variable in the message
* set a map as your message payload
* generate an output based on evaluating the input within DataMapper
* access an inbound property
* dynamically set the payload

#####Flow 5 ??? Using Xpath

In all the previous flows, calls to the service were made via GET requests that included query parameters. In this example, the service you create is an API that accepts POST requests with XML bodies. The required XML includes two parameters, username and age. The service stores these two parameters and adds a third boolean parameter that evaluates if the user is above a certain age (if age >18)

In this flow, you use MEL to:

* set a flow variable in the message
* generate an output based on evaluating the input
* parse an XML input through an xpath query
* dynamically set the payload

 

#####Flow 6 ??? Working with Java Objects

This flow is just like flow 5, except that the service now receives JSON inputs rather than of XML.

The JSON input includes two parameters, username and age. The service stores these two parameters and adds a third boolean parameter that evaluates if the user is above a certain age (if age>18). Mule first transforms the JSON object into a Java object so that MEL expressions can access the object's attributes.

In this example, you will use MEL to:

* set a flow variable in the message
* generate an output based on evaluating the input
* access a Java object's attributes
* dynamically set the payload

###Go further

* Read about how to build this example [here](http://www.mulesoft.org/documentation/display/current/Mule+Expression+Language+Examples).

* Access the Mule Expression Language [Reference](http://www.mulesoft.org/documentation/display/current/Mule+Expression+Language+Reference) and [Tips](http://www.mulesoft.org/documentation/display/current/Mule+Expression+Language+Tips).

* Want to learn more about how to get information about the Mule message so that you can work with it using MEL? Try out this [tutorial](http://www.mulesoft.org/documentation/display/current/Mule+Message+Tutorial). 

