* In error response add a key to the path and timestamp. This makes it easier to find the logs and stacktrace associated with it.

* Always return an object from api, that way we can add more keys without breaking the client code.

* To perform search through multiple tables in the database create a view.

* The value associated with the `message` key should be stored as a constant:

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

* Specify `@AllArgsConstructor` to services and controller to avoid constructor injection.

* When to use `@Modifying` annotation?

* Suppose you are hitting an external API to fetch some data. If the API is down, your request might fail. A more fail-safe approach would be to push the message to a message queue (MQ) and create a consumer service. The consumer service reads the data from the MQ and attempts to hit the external API. If successful, the message is removed from the MQ; otherwise, the consumer will read the message again and retry hitting the external API.. 

* To build the application and run tests use:

```bash
./mvnw clean install
```

* To run the application execute the following command:

```bash
./mvnw spring-boot:run
```

or we can use `java -jar` command:

```bash
.java -jar app.jar
```

* Docker containerization is done using google jib. Add dependency to `pom.xml` and run the following to create the build:

```bash
./mvnw compile jib:dockerBuild
```

or use the following to build and push to docker hub:

```bash
./mvnw compile jib:build
```


* To read environment variables inside spring boot application use `Environment` interface.

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

* Http status code for "no content" is 204.

* This command pulls the images and then creates the containers:

```bash
docker compose pull && docker compose up
```

* We can validate @Path variable using the @Pattern annotation as follows:

```java
public Response<CommonResDto> fetchCustomer(@Pathvariable 
@Pattern(regexp="^[0-9]{10}", message="mobile no. must be 10 digits long")
String mobileNumber){
     // ...
}
```

* The return type of `HttpHeaders.get(key)` is `List<String>`.

* To access query params in ccontroller methods use `@RequestParam`. Similarly, to access headers in controller methods use `@RequestParam`.

* Since Java 19, ExecutoService implements AutoClose.

* TTL in DNS specifies the time for which the domain name will be changed by the client. Here is the output of the `dig` command.

```bash
prateek@WKWEJ772644041:~$ dig overiq.com

; <<>> DiG 9.18.18-0ubuntu0.22.04.1-Ubuntu <<>> overiq.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 43806
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;overiq.com.                    IN      A

;; ANSWER SECTION:
overiq.com.             55      IN      A       3.7.161.16

;; Query time: 19 msec
;; SERVER: 10.255.255.254#53(10.255.255.254) (UDP)
;; WHEN: Mon Aug 26 12:09:30 IST 2024
;; MSG SIZE  rcvd: 55
```

TTL can be seen in `ANSWER SECTION:` of the above command output followed by the domain name. If you run the command repeatedly, you will see it it decrementing.

* We can use single gmail account to create multiple accounts for aws. For exmaple: if your email is `java@gmail.com`, then `java@gmail+01.com`, `java@gmail+02.com` etc and so on can be used to create new accounts. Note that any email to sent to `java@gmail+01.com` will be forwarded to `java@gmail.com` only.


