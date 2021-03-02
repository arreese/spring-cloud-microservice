## JpaRepository
- The entity that we want to manage is CurrencyExchange and ID(type) is Long
```
public interface CurrencyExchangeRepository extends JpaRepository<CurrencyExchange, Long> {
}
```

## Circuit Breaker

### Dependency
```
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-spring-boot2</artifactId>
</dependency>
```

### States 
- Closed : When you are calling the dependent microservice continuously
- Open : The circuit breaker will not call the microservice, it will directly return the fallback response
- Half Open : The circuit beaker will be sending a percentage of requests to the dependent microservice, and the 
  rest of the request will return hardcoded response or the fallback response
  
### Rate Limiting
- Saying: in 10 seconds, I would want to only allow 10000 calls to the sample api
- Can be to a specific API

File: application.properties
```
// How many would you want to allow? 2 requestes in every 10 seconds
resilience4j.ratelimiter.instances.default.limit-for-period=2
resilience4j.ratelimiter.instances.default.limit-refresh-period=10s
```

### BulkHead
- Configuring how many concurrent calls are allowed