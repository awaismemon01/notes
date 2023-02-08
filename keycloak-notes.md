# Keycloak

- Keycloak is essentially an executable jar made using Quarkus-Java.
- Keycloak follows the Open-Closed Principle
  - artifacts should be extendible without any modification
- We can provide our own custom implementation of services in keycloak

- To make keycloak use our custom implementation :
  - There is a META-INF/services directory present.
  - Make a file with the name being the fully qualified name if the interface we are implementing
    > if the interface I am implementing is ``` com.example.CodecSet ```  
    > then the file should look like this `META-INF/services/com.example.CodecSet`  
    > the file should have the fully qualified name of our implementing class like `com.example.service.StandardCodecSet`  

- One more prerequiste is that the class should have a no-args constructor (still not clear properly why)


## Provider Patterns
Keycloak has several Abstract Provider and Abstract Provider Factory classes.  
To implement our SPI we need to extend one of those Abstract classes and **NOT THE CONCRETE CLASSES** because using concrete classes may lead to the service loader not picking up our services despite doing all the configurations correctly.
