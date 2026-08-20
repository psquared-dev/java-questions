### Creational

* Singleton
* Factory Method
* Abstract Factory
* Builder

### Structural

* Adapter
* Decorator
* Facade
* Proxy

### Behavioral

* Strategy
* Observer
* Chain of Responsibility
* Template Method
* Command
* Visitor

## Singleton Design Pattern

## Approach 1

```java
package practice;

public class Config {
	private static Config instance;

	private Config() {
	}

	public static synchronized Config getInstance() {
		if (instance == null) {
			instance = new Config();
		}

		return instance;
	}
}
```

## Approach 2

```java
package practice;

public class Config {
	private static volatile Config instance;

	private Config() {
	}

	public static Config getInstance() {
		if (instance == null) {
			synchronized (Config.class) {
				if (instance == null) {
					instance = new Config();
				}
			}
		}

		return instance;
	}
}
```

-----------------------

## Factory Pattern

```java
interface PaymentMethod {
	void pay(String details);
}

class UPIPayment implements PaymentMethod {

	@Override
	public void pay(String details) {
		System.out.println(this.getClass().getName());
	}
}

class CreditCardPayment implements PaymentMethod {

	@Override
	public void pay(String details) {
		System.out.println(this.getClass().getName());
	}
}

class DebitCardPayment implements PaymentMethod {

	@Override
	public void pay(String details) {
		System.out.println(this.getClass().getName());
	}
}

class PaymentFactory {
	public static PaymentMethod getPayment(String type) {
		return switch (type) {
			case "UPI" -> new UPIPayment();
			case "CREDIT" -> new CreditCardPayment();
			case "DEBIT" -> new DebitCardPayment();
			default -> throw new IllegalArgumentException();
		};
	}
}

public class Tmp {
	public static void main(String[] args) {

		PaymentMethod upi = PaymentFactory.getPayment("UPI");
		upi.pay("11");

		PaymentMethod credit = PaymentFactory.getPayment("CREDIT");
		credit.pay("12");
	}
}
```

## Abstract Factory

```java
package practice;

// ==========================================
// 1. Abstract Product Interfaces (Product Family)
// ==========================================
interface Storage {
	boolean store(String filename);
}

interface Compute {
	void run(String task);
}

// ==========================================
// 2. Concrete Products (AWS Implementation Family)
// ==========================================
class AWSS3 implements Storage {
	@Override
	public boolean store(String filename) {
		System.out.println("[AWS S3] Uploading file: " + filename);
		return true;
	}
}

class EC2Instance implements Compute {
	@Override
	public void run(String task) {
		System.out.println("[AWS EC2] Running task on virtual machine: " + task);
	}
}

// ==========================================
// 3. Concrete Products (GCP Implementation Family)
// ==========================================
class GCPStorage implements Storage {
	@Override
	public boolean store(String filename) {
		System.out.println("[GCP Cloud Storage] Uploading bucket object: " + filename);
		return true;
	}
}

class ComputeEngine implements Compute {
	@Override
	public void run(String task) {
		System.out.println("[GCP Compute Engine] Running task on Google instance: " + task);
	}
}

// ==========================================
// 4. Abstract Factory Interface
// ==========================================
interface CloudFactory {
	Storage createStorage();

	Compute createCompute();
}

// ==========================================
// 5. Concrete Factories
// ==========================================
class AWSCloudFactory implements CloudFactory {
	@Override
	public Storage createStorage() {
		return new AWSS3();
	}

	@Override
	public Compute createCompute() {
		return new EC2Instance();
	}
}

class GCPCloudFactory implements CloudFactory {
	@Override
	public Storage createStorage() {
		return new GCPStorage();
	}

	@Override
	public Compute createCompute() {
		return new ComputeEngine();
	}
}

// ==========================================
// 6. Client Service
// ==========================================
class CloudDeploymentService {
	private final Storage storage;
	private final Compute compute;

	public CloudDeploymentService(CloudFactory cloudFactory) {
		this.storage = cloudFactory.createStorage();
		this.compute = cloudFactory.createCompute();
	}

	public void deployApp(String artifactName, String taskName) {
		storage.store(artifactName);
		compute.run(taskName);
	}
}

// ==========================================
// 7. Execution Entry Point
// ==========================================
public class Tmp {
	public static void main(String[] args) {
		System.out.println("=== Deploying to AWS ===");
		CloudFactory awsFactory = new AWSCloudFactory();
		CloudDeploymentService awsService = new CloudDeploymentService(awsFactory);
		awsService.deployApp("app.jar", "Database Migration Task");

		System.out.println("\n=== Deploying to GCP ===");
		CloudFactory gcpFactory = new GCPCloudFactory();
		CloudDeploymentService gcpService = new CloudDeploymentService(gcpFactory);
		gcpService.deployApp("app.tar.gz", "ETL Processing Task");
	}
}
```

-----------------------

## Builder Pattern

```java
package practice;

public class User {
	private final String userName;
	private final String email;
	private final int age;

	private User(Builder builder) {
		this.userName = builder.userName;
		this.email = builder.email;
		this.age = builder.age;
	}

	public String getUserName() {
		return userName;
	}

	public String getEmail() {
		return email;
	}

	public int getAge() {
		return age;
	}

	public static class Builder {
		private String email;
		private String userName;
		private int age;

		public Builder() {
		}

		public Builder email(String email) {
			this.email = email;
			return this;
		}

		public Builder userName(String userName) {
			this.userName = userName;
			return this;
		}

		public Builder age(int age) {
			this.age = age;
			return this;
		}

		public User build() {
			return new User(this);
		}
	}

}


class Tmp {
	public static void main(String[] args) {
		// Case 1: Building with all properties chained
		User user1 = new User.Builder()
				.userName("john_doe")
				.email("john@example.com")
				.age(28)
				.build();

		// Case 2: Building with optional fields omitted
		User user2 = new User.Builder()
				.userName("alice_w")
				.email("alice@example.com")
				.build();

		System.out.println("User 1: " + user1);
		System.out.println("User 2: " + user2);
		System.out.println("User 1 Age: " + user1.getAge());
	}
```

-----------------------

## Decorator Pattern

```java
interface Notification {
	void send(String message);
}

abstract class BaseNotification implements Notification {
	public Notification notification;

	public BaseNotification() {
	}

	public BaseNotification(Notification notification) {
		this.notification = notification;
	}

	@Override
	public void send(String message) {
		if (notification != null) {
			notification.send(message);
		}
	}
}

class EmailNotification extends BaseNotification {
	public EmailNotification() {
	}

	public EmailNotification(Notification notification) {
		super(notification);
	}

	@Override
	public void send(String message) {
		super.send(message);
		System.out.println(this.getClass().getSimpleName());
	}
}

class SMSNotification extends BaseNotification {
	public SMSNotification() {
	}

	public SMSNotification(Notification notification) {
		super(notification);
	}

	@Override
	public void send(String message) {
		super.send(message);
		System.out.println(this.getClass().getSimpleName());
	}
}

public class Tmp {
	public static void main(String[] args) {
		Notification notification = new SMSNotification(new EmailNotification());
		notification.send("message");
	}
}

```

-----------------------

## Chain of Responsibility Pattern

```java
interface RequestFilter {
	void handle(String request);

	RequestFilter setNext(RequestFilter next);
}

abstract class AbstractRequestFilter implements RequestFilter {
	protected RequestFilter next;

	public AbstractRequestFilter() {
	}

	public RequestFilter setNext(RequestFilter next) {
		if (this.next == null) {
			this.next = next;
		} else {
			this.next.setNext(next);
		}
		return next;
	}

	@Override
	public void handle(String request) {
		if (next != null) {
			next.handle(request);
		}
	}
}

class IpFilter extends AbstractRequestFilter {
	@Override
	public void handle(String request) {
		System.out.println(this.getClass().getSimpleName());
		super.handle(request);
	}
}

class AuthenticationFilter extends AbstractRequestFilter {
	@Override
	public void handle(String request) {
		System.out.println(this.getClass().getSimpleName());
		super.handle(request);
	}
}

class GatewayFilter extends AbstractRequestFilter {
	@Override
	public void handle(String request) {
		System.out.println(this.getClass().getSimpleName());
		super.handle(request);
	}
}

public class Tmp {
	public static void main(String[] args) {
		RequestFilter requestFilter = new IpFilter();
		requestFilter.setNext(new AuthenticationFilter());
		requestFilter.setNext(new GatewayFilter());


		requestFilter.handle("1234");
	}
}
```