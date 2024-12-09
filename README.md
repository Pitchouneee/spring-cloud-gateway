# Spring Cloud Gateway with Eureka Discovery

## Description

### Microservices architecture

This project implements an API Gateway using Spring Cloud Gateway, designed to:
- Provide dynamic routing between microservices
- Centralize security management
- Handle inter-service request routing

### Key components

- **Spring Cloud Gateway**: Request routing and filtering
- **Eureka Discovery**: Service registration and discovery
- **OAuth2/Keycloak**: Centralized authentication and authorization

## Technical configuration

### Eureka configuration
```yaml
eureka:
  client:
    region: eu-west-1
    serviceUrl:
      defaultZone: ${EUREKA_URI:http://host.docker.internal:8761/eureka}
```
- Dynamic service registration
- Configured region (eu-west-1)
- Customizable URI via environment variable

### Dynamic Routing
```yaml
spring:
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true
          lower-case-service-id: true
```
- Automatic service discovery
- Convert service IDs to lowercase
- Service-name based routing

### OAuth2 security
```yaml
security:
  oauth2:
    resourceserver:
      opaquetoken:
        introspection-uri: http://host.docker.internal:9000/realms/dev/protocol/openid-connect/token/introspect
      jwt:
        jwk-set-uri: http://host.docker.internal:9000/realms/dev/protocol/openid-connect/certs
```
- Token validation via Keycloak
- JWT introspection and verification
- Multi-environment configuration

### CORS
```yaml
spring:
  cloud:
    gateway:
      globalcors:
        add-to-simple-url-handler-mapping: true
        cors-configurations:
          '[/**]':
            allowed-origins: "*"
            allowed-methods: "*"
            allowed-headers: "*"
```
- Global CORS configuration
- Permissive settings (to be restricted in production)

## Prerequisites

### Environment

- Java 17+
- Spring Boot 3.x
- Spring Cloud 2022.x
- Docker (recommended)

### Dependencies

- spring-cloud-starter-gateway
- spring-cloud-starter-netflix-eureka-client
- spring-boot-starter-oauth2-resource-server

## Security recommendations

### Configuration
- Restrict CORS origins
- Use external secrets
- Implement custom security filters

### Best practices
- Limit OAuth2 permissions
- Enable HTTPS
- Access logging
- Fine-grained role management

## Customization

### Routing Extension
- Add custom predicates
- Implement request/response filters
- Handle routing errors

## Contribution

Contributions are welcome ! If you want to report an issue or suggest feature, feel free to open an issue or submit a pull request