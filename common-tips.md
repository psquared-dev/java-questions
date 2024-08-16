1. In error response add a key to the path and timestamp. This makes it easier to find the logs and stacktrace associated with it.

1. Always return an object from api, that way we can add more keys without breaking the client code.

1. To perform search through multiple tables in the database create a view.

1. The value associated with the `message` key should be stored as a constant:

```json
{
    "data": {
        "id": "13ab10e5-103d-4d09-93f9-72d5830971a9",
        "firstName": "f1",
        "lastName": "l1",
        "email": "aa@mail.com"
    },
    "message": "User fetched",
    "status": "SUCCESS"
}
```

1. Specify `@AllArgsConstructor` to services and controller to avoid constructor injection.

1. When to use `@Modifying` annotation?

1. Suppose you are hitting an external API to fetch some data. If the API is down, your request might fail. A more fail-safe approach would be to push the message to a message queue (MQ) and create a consumer service. The consumer service reads the data from the MQ and attempts to hit the external API. If successful, the message is removed from the MQ; otherwise, the consumer will read the message again and retry hitting the external API.. 

1. To build the application and run tests use:

```bash
./mvnw clean install
```

1. To run the application execute the following command:

```bash
./mvnw spring-boot:run
```

or we can use `java -jar` command:

```bash
.java -jar app.jar
```

1. Docker containerization is done using google jib. Add dependency to `pom.xml` and run the following to create the build:

```bash
./mvnw compile jib:dockerBuild
```

or use the following to build and push to docker hub:

```bash
./mvnw compile jib:build
```


1. To read environment variables inside spring boot application use `Environment` interface.

```java
import org.springframework.core.env.Environment;

public UserController(UserService userService, Environment environment) {
        this.userService = userService;
        this.environment = environment;
    }
    
    @GetMapping("{id}")
    public ResponseEntity<CommonResDto> getUser(@PathVariable UUID id) {
        log.info("{}", environment.getProperty("USER"));
        UserResDto user = userService.getUser(id);
        CommonResDto userFetched = new CommonResDto(user, "User fetched");
        return ResponseEntity.ok(userFetched);
    }
```

1. Http status code for "no content" is 204.

1. This command pulls the images and then creates the containers:

```bash
docker compose pull && docker compose up
```

1. We can validate @Path variable using the @Pattern annotation as follows:

```java
public Response<CommonResDto> fetchCustomer(@Pathvariable 
@Pattern(regexp="^[0-9]{10}", message="mobile no. must be 10 digits long")
String mobileNumber){
     // ...
}
```

1. The return type of `HttpHeaders.get(key)` is `List<String>`.

1. To access query params in ccontroller methods use `@RequestParam`. Similarly, to access headers in controller methods use `@RequestParam`.
