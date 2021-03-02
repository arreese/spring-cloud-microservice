# Spring Cloud Gateway
- Simple, effective wat to route to APIs
- Provide cross cutting concerns
    - security
    - monitoring/metrics
- Build on top of Spring WebFlux
- Features
    - Match routes on any request attribute
    - Define Predicates and Filers
    - Integrates with Spr9ing Cloud Discovery Client(Load Balancing)
    - Path Rewriting



## API Gateway
- http://localhost:8765/CURRENCY-EXCHANGE/currency-exchange/from/USD/to/INR

- http://localhost:8765/CURRENCY-CONVERSION/currency-conversion/from/USD/to/INR/quantity/10

- http://localhost:8765/CURRENCY-CONVERSION/currency-conversion-feign/from/USD/to/INR/quantity/10 
- Call any microservice which is registered with Eureka through teh API Gateway
  if you want to implement things like Authentication, you can implement them on API
  Gateway and only allow those things which are authenticated in your microservices
  