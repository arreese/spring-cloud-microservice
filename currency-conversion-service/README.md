## Proxy

### Feign Dependency
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```
- Makes it easier to call API

### Annotations
- @EnableFeignClients in main Application file
- @FeignClient in Proxy file
    - @FeignClient(name = "currency-exchange", url="localhost:8000")

```
@RestController
public class CurrencyConversionController {

    @Autowired
    private CurrencyExchangeProxy proxy;

    //directly calls another microservice
    @GetMapping("/currency-conversion/from/{from}/to/{to}/quantity/{quantity}")
    public CurrencyConversion currencyConversion(
            @PathVariable String from,
            @PathVariable String to,
            @PathVariable BigDecimal quantity){
        HashMap<String, String> uriVariables = new HashMap<>();
        uriVariables.put("from", from);
        uriVariables.put("to", to);

        ResponseEntity<CurrencyConversion> repsonseEntity = new RestTemplate().getForEntity
                ("http://localhost:8000/currency-exchange/from/{from}/to/{to}",
                        CurrencyConversion.class, uriVariables);
        CurrencyConversion currencyConversion = repsonseEntity.getBody();
        return new CurrencyConversion(currencyConversion.getId(), from, to, quantity,
                currencyConversion.getConversionMultiple(),
                quantity.multiply(currencyConversion.getConversionMultiple()), currencyConversion.getEnvironment() + " " + "rest template");
    }
    //Uses a proxy to call a microservice
    @GetMapping("/currency-conversion-feign/from/{from}/to/{to}/quantity/{quantity}")
    public CurrencyConversion calculateCurrencyConversion(
            @PathVariable String from,
            @PathVariable String to,
            @PathVariable BigDecimal quantity){

        CurrencyConversion currencyConversion = proxy.retrieveExchangeValue(from, to);
        return new CurrencyConversion(currencyConversion.getId(), from, to, quantity,
                currencyConversion.getConversionMultiple(),
                quantity.multiply(currencyConversion.getConversionMultiple()), currencyConversion.getEnvironment() + " " + " feign");
    }
}
```

## To get registered with Eureka(Naming-server)
- Add the following dependency to pom.xml
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```