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
    > and the content of the file should have the fully qualified name of our implementing class or classes like `com.example.service.StandardCodecSet`  

- One more prerequiste is that the class should have a no-args constructor (still not clear properly why)


## Provider Patterns
Keycloak has several Abstract Provider and Abstract Provider Factory classes.  
To implement our SPI we need to extend one of those Abstract classes and **NOT THE CONCRETE CLASSES** because using concrete classes may lead to the service loader not picking up our services despite doing all the configurations correctly.

## Scopes
KeyCloak session is request scoped so every request creates a new session.

Provider Factory is Application scoped, i.e. one instance is created throughout the life of the Application

Providers are created on demand, session-scoped


## For Future Reference
DefaultEmailSenderProviderFactory possible lead for the email verification

KeyCloak only allows one provider implementation for some SPIs such as Email provider, Database Provider, etc.
