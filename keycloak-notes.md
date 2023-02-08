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

