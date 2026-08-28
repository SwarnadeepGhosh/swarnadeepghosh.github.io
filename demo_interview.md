# Demo Interview Questions

## General Questions

**1. Tell me about yourself.**
I'm a backend Java developer with 6 years of experience, currently a Senior Associate at PwC India working on insurance platform backends using Java, Spring Boot, JPA, and PostgreSQL. Before this, I worked at TCS on SBI Card's credit card origination system. Alongside backend work, I've picked up DevOps skills — Jenkins, Docker, Azure VM deployments — and I also build Angular frontends when needed. I'm B.Tech in ECE from MAKAUT.

**2. Explain your roles and responsibilities.**
At PwC, I build and maintain backend services for an insurance platform (Java, Spring Boot, JPA, Microservices, Feign Client), and I also built a Recruitment Tracking system end-to-end using Spring Security and MSSQL. At TCS, I worked on SBI Card's Sales24 platform — fixing production bugs, building new features, and automating manual processes like source-code mapping (cut effort by 95%) using Angular.

**3. Explain your Internet Banking project.**
This maps closely to my SBI Card Sales24 work — the credit card origination journey: customer onboarding, CIBIL score check, digital KYC, and card issuance, built with Java, Spring Boot, JPA, Hibernate, and REST microservices.

**4. What architecture did you use?**
Microservices architecture — separate services per business capability (e.g., order, payment), communicating via REST/Feign Client, with each service owning its own database.

**5. Did you use Spring Boot and Microservices?**
Yes — Spring Boot for the services themselves, and a microservices architecture with components like Eureka (service discovery), API Gateway, and Feign Client for inter-service calls (used in my AI Fitness App and insurance platform).

**6. Did you use JWT?**
Yes — in my Todoify project, I implemented JWT-based authentication with Spring Security, where a logged-in user's token is validated on every request via a custom filter.

---

## JAVA Core Concepts

**7. Difference between JVM, JDK, and JRE.**

- JVM = runtime engine that executes bytecode. 
- JRE = JVM + libraries needed to run Java apps. 
- JDK = JRE + development tools (compiler, debugger). 
- Formula: `JDK = JRE + dev tools`, `JRE = JVM + libraries`.
- **Simple analogy:** JVM = engine, JRE = Car (engine + fuel), JDK = full car factory (JRE + tools to build).

**8. Difference between == and equals().**

```java
String s1 = new String("hello");
String s2 = new String("hello");
System.out.println(s1 == s2);       // false — compares references
System.out.println(s1.equals(s2));  // true — compares content
```

`==` compares memory references (object identity); `equals()` compares actual content (when overridden, like in `String`).

For plain `Object`, `equals()` defaults to `==` unless overridden. `String` overrides `equals()` to compare content.

**9. String Pool.**

```java
String a = "hello";
String b = "hello";
System.out.println(a == b); // true — both point to same pooled object
```

Java caches string literals in a special memory area (String Constant Pool) — identical literals reuse the same object instead of creating duplicates, saving memory.

**10. String, StringBuilder, and StringBuffer.**

- `String` is immutable — every modification creates a new object. 
- `StringBuilder` is mutable and fast but not thread-safe. 
- `StringBuffer` is mutable and thread-safe (synchronized) but slower. 
- I use `StringBuilder` by default, `StringBuffer` only when multiple threads modify the same instance.

**11. `ABC abc = new ABC();` — stack memory and Java memory management.**
`abc` (the reference variable) lives on the **stack**. The actual `ABC` object lives on the **heap**. The stack variable just holds the memory address pointing to the heap object. When `abc` goes out of scope, the stack reference is removed, and the heap object becomes eligible for garbage collection if nothing else references it.

**12. Constructor.**
A special method matching the class name, with no return type, called automatically when an object is created via `new` — used to initialize the object's state.

```java
class Employee {
    String name;
    Employee(String name) { this.name = name; } // constructor
}
```

**13. Wrapper class.**
A class that wraps a primitive type into an object — e.g., `int → Integer`, `double → Double`. Needed because Collections (List, Map) can only store objects, not primitives.

```java
Integer i = 10; // autoboxing: int → Integer
int j = i;      // unboxing: Integer → int
```

**14. static keyword and block.**
`static` members belong to the class, not an instance — shared across all objects. A **static block** runs once when the class is loaded, before `main()`, useful for one-time static initialization.

```java
class Config {
    static int version;
    static { version = 1; System.out.println("Static block executed"); }
}
// Output (on first access to Config class): Static block executed
```

**15. Aggregation vs Association.**

- **Association** = a general relationship between two independent classes (e.g., `Teacher` and `Student`, both can exist without each other). 
- **Aggregation** = a special "has-a" association where one object contains a reference to another, but the contained object can still exist independently (e.g., `Department` has `Employee`s, but an `Employee` can exist without that `Department`).

---

## Collections Framework

**16. Collections used in your project.**

- `List` (ArrayList) for ordered API response data, 
- `Map` (HashMap) for quick lookups, 
- `Set` for uniqueness checks (e.g., deduplicating IDs), and 
- Spring Data's `Page`/`Pageable` for paginated results.

**17. Collections — ArrayList, HashMap, HashSet, ConcurrentHashMap, TreeMap, LinkedHashMap.**

- `ArrayList` = fast random access, dynamic array. 
- `HashMap` = key-value, no order, fast lookup. 
- `HashSet` = unique elements, no order (backed by HashMap). 
- `ConcurrentHashMap` = thread-safe HashMap for concurrent access. 
- `TreeMap` = key-value, sorted by key. 
- `LinkedHashMap` = key-value, preserves insertion order.

**18. HashSet vs ConcurrentHashMap.**

- `HashSet` stores unique elements, not thread-safe, backed internally by a `HashMap`. 
- `ConcurrentHashMap` is a thread-safe **Map** (key-value), using segment/bucket-level locking so multiple threads can read/write concurrently without locking the whole map.

**19. Load Factor in HashMap.**
Default is 0.75 — when the map is 75% full (based on capacity), it automatically resizes (doubles) to reduce collisions and maintain O(1) performance. Default initial capacity is 16.

**20. Which collection preserves insertion order?**
`LinkedHashSet` and `LinkedHashMap` — they use an internal linked list alongside hashing to remember insertion order, unlike plain `HashSet`/`HashMap`.

**21. Best collection for sorting.**
`TreeSet` (unique + sorted) or `TreeMap` (key-value + sorted by key) — both backed by a Red-Black Tree, giving natural ascending order automatically with O(log n) operations.

**22. HashMap internal working**

<img src="img/hashmap-working-3.png" alt="image" style="zoom: 64%;" />

- *What it is:* 

  - `HashMap` stores key-value pairs in an internal **array of buckets**. Where an entry lands is decided by the key's `hashCode()`, not by insertion order — that's why HashMap has no guaranteed ordering.

  - *HashMap uses an array of buckets — a key's hashCode is converted into a bucket index via `hash & (capacity-1)`, giving O(1) average lookup. When two keys collide into the same bucket, they're chained as a linked list (or a red-black tree if the chain grows past 8 entries in Java 8+), which is why a poorly distributed hashCode can degrade performance to O(n).*

  - HashMap contains an HashTable(array of Node) and Node can represent a class having following objects :

    ```java
    Node {
        int hash
        K key
        V value
        Node next
    }
    ```

- **Why Hashmap is most powerful ?**

  - It gives O(1) complexity for get, insert, delete operation

- *How `put(key, value)` works*

  1. Java computes `key.hashCode()`
  2. That hash is converted into an **array index** using `hash & (capacity - 1)` — a fast way to map any hash into a valid bucket index
  3. The entry is placed in that bucket

- *What happens on a collision*

  - If two different keys hash to the **same bucket index** (a "collision" — like `apple` and `banana` both landing on index 2 above), Java doesn't overwrite one — it **chains** them together:
    - In Java 7 and earlier: always a **linked list** in that bucket.
    - In Java 8+: starts as a linked list, but if a single bucket gets too crowded (8+ entries) it converts to a **self balancing binary search tree** for that bucket, so worst-case lookup becomes O(log n) instead of O(n)

- *How `get(key)` works*

  1. Compute the same `hashCode()` → same bucket index
  2. Jump directly to that bucket (O(1))
  3. If multiple entries are chained there, walk the chain and use `equals()` to find the exact matching key

- *Why `equals()` and `hashCode()` must be consistent*

  - If two keys are `equal` but have **different hashCodes**, they might land in different buckets — `get()` would look in the wrong bucket and fail to find the value, even though logically it should exist.

  - ```java
    Map<String, Integer> map = new HashMap<>();
    map.put("apple", 5);
    map.put("banana", 2); // suppose this collides into the same bucket as "apple"
    
    System.out.println(map.get("apple"));  // 5 — jumps to bucket, walks chain, matches via equals()
    ```

- *Key facts to remember*

  - | Fact                         | Value                                               |
    | ---------------------------- | --------------------------------------------------- |
    | Default initial capacity     | 16                                                  |
    | Default load factor          | 0.75 (resizes/doubles capacity when 75% full)       |
    | Average time complexity      | O(1) for get/put                                    |
    | Worst case (many collisions) | O(n) with linked list, O(log n) with tree (Java 8+) |


**23. hashCode/equals contract, breaking it, fixed hashCode effect.**
Contract: equal objects **must** have equal hashCodes. Breaking it (e.g., overriding `equals()` but not `hashCode()`) causes `HashMap`/`HashSet` to treat logically equal objects as different, so lookups fail. If `hashCode()` always returns the same fixed value, **all keys land in one bucket** — the map degrades to a linked list, turning O(1) operations into O(n).

**24. Concurrent collections internal working.**

- `ConcurrentHashMap` uses fine-grained locking (bucket/segment level in older versions, CAS + synchronized blocks per bin in Java 8+) so multiple threads can read/write different parts concurrently. `CopyOnWriteArrayList` creates a **new copy of the entire array** on every write, so reads never block — good for read-heavy, rarely-written lists.
- Reference 1: [Thread Safe Arraylist, LinkedList](https://tutorials-sg.vercel.app/interview/Java_Interview_Questions.html#thread-safe-arraylist-linkedlist)
- Reference 2: [ConcurrentHashMap](https://tutorials-sg.vercel.app/interview/Java_Interview_Questions.html#hashmap-vs-concurrenthashmap)

---

## Java 8 & Functional Programming

**25. Stream API to map Employee to Employee Name.**

```java
List<String> names = employees.stream()
        .map(Employee::getName)
        .collect(Collectors.toList());
System.out.println(names);
// Output: [John, Jane, Bob]
```

**26. Method Reference (::) vs Lambda.**

```java
list.forEach(e -> System.out.println(e));   // lambda
list.forEach(System.out::println);          // method reference — same result, cleaner
```

Method reference is shorthand for a lambda that just calls an existing method — same behavior, more readable when no extra logic is needed.

**27. Java Stream — filter, map, flatMap, sorting, Comparator, groupBy, collect.**

```java
// Getting name of employees having salary > 50000 in sorted manner
List<String> names = employees.stream()
    .filter(e -> e.getSalary() > 50000)                      // keep matching elements
    .map(Employee::getName)                                   // transform each element
    .sorted(Comparator.naturalOrder())                        // sort
    .collect(Collectors.toList());                            // terminal operation → List

Map<String, List<Employee>> byDept = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDepartment)); // group by a key, {Finance=[David], HR=[Bob, Eve], IT=[Alice, Charlie]}

List<Integer> flat = List.of(List.of(1,2), List.of(3,4)).stream()
    .flatMap(List::stream)   // flattens nested lists into one stream
    .collect(Collectors.toList());
System.out.println(flat); // Output: [1, 2, 3, 4]
```

**28. Department-wise Top 3 Highest Salaries using Stream API.**

```java
Map<String, List<Employee>> top3ByDept = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDepartment,
        Collectors.collectingAndThen(
            Collectors.toList(),
            list -> list.stream()
                .sorted(Comparator.comparing(Employee::getSalary).reversed())
                .limit(3)
                .collect(Collectors.toList())
        )));
```

Groups employees by department, then within each group sorts by salary descending and keeps top 3.

---

## Coding Exercises

**29. Reverse words in a string.**

```java
String input = "backend java developer";
String[] words = input.split(" ");
Collections.reverse(Arrays.asList(words));
String result = String.join(" ", words);
System.out.println(result);
// Output: developer java backend
```

**30. Time and Space Complexity.**
Time complexity = how execution time grows with input size (e.g., O(n) for a loop, O(log n) for binary search). Space complexity = how much extra memory an algorithm uses relative to input size. Used to compare algorithm efficiency independent of hardware.

**31. First non-repeating character index.**

```java
public static int firstNonRepeating(String s) {
    Map<Character, Integer> count = new LinkedHashMap<>();
    for (char c : s.toCharArray()) count.merge(c, 1, Integer::sum);
    for (int i = 0; i < s.length(); i++) {
        if (count.get(s.charAt(i)) == 1) return i;
    }
    return -1;
}
System.out.println(firstNonRepeating("abcdbaf")); // Output: 2
System.out.println(firstNonRepeating("abcjhg"));  // Output: -1
```

---

## Multithreading & Concurrency

**32. Multithreading.**
Running multiple threads concurrently within a single program, sharing the same memory space (unlike processes), allowing tasks to execute in parallel and better utilize CPU cores.

**33. How to create a thread.**

```java
// Way 1: extend Thread
class MyThread extends Thread {
    public void run() { System.out.println("Running"); }
}
new MyThread().start();

// Way 2: implement Runnable (preferred)
Runnable task = () -> System.out.println("Running via Runnable");
new Thread(task).start();
```

**34. ExecutorService.**

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
executor.submit(() -> System.out.println("Task running"));		// Output: Task running
executor.shutdown();
```

Manages a pool of reusable threads so you don't manually create/destroy threads for every task — reduces overhead and resource thrashing.

**35. Callable vs Future vs CompletableFuture.**

- `Callable` = a task that returns a result (`V call()`). 
- `Future` = a placeholder for a result that will be ready later (`.get()` blocks until done). 
- `CompletableFuture` = an enhanced `Future` that supports chaining callbacks (`.thenApply()`) without blocking.

**36. When would you use CompletableFuture?**
When I need to run multiple independent tasks in parallel and combine their results — e.g., calling 3 different microservices simultaneously instead of sequentially, then merging responses once all complete.

```java
CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> 10 + 20, executor)
    .thenApply(result -> result * 2);
System.out.println(future.get());	 // Output: 60
```

**37. Class-level and method-level locking.**
**Method-level** (`synchronized` on an instance method) locks on `this` — only one thread can execute that method on that object at a time. **Class-level** (`synchronized` on a static method, or `synchronized(MyClass.class)`) locks on the Class object itself — affects all instances, since static methods aren't tied to one object.

**38. Two threads alternately printing odd and even numbers (1–100) using wait()/notify().**

```java
class Printer {
    private int num = 1;
    private final int max = 100;
    synchronized void printOdd() {
        while (num <= max) {
            while (num % 2 == 0) { try { wait(); } catch (InterruptedException e) {} }
            System.out.println("Odd: " + num++);
            notify();
        }
    }
    synchronized void printEven() {
        while (num <= max) {
            while (num % 2 != 0) { try { wait(); } catch (InterruptedException e) {} }
            System.out.println("Even: " + num++);
            notify();
        }
    }
}
Printer p = new Printer();
new Thread(p::printOdd).start();
new Thread(p::printEven).start();
// Output: Odd: 1, Even: 2, Odd: 3, Even: 4 ... up to 100
```

---

## Garbage Collection & JVM

**39. Who performs Garbage Collection?**
The JVM — specifically, a background GC thread/process automatically reclaims memory occupied by objects no longer reachable from any live reference.

**40. When does an object become eligible for Garbage Collection?**
When it has **no live references** pointing to it — e.g., a local variable going out of scope, or explicitly setting a reference to `null` with no other references remaining.

**41. Can JVM run C/C++ code?**
Not directly — but via **JNI (Java Native Interface)**, Java code can call native C/C++ functions, and vice versa, bridging managed Java code with native libraries.

---

## Design Patterns

**42. Thread-safe Singleton (enhanced — non-clonable, reflection-safe, serialization-safe).**

```java
public class Singleton implements Serializable {
    private static volatile Singleton instance;
    private Singleton() {
        if (instance != null) throw new RuntimeException("Use getInstance()"); // block reflection
    }
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) instance = new Singleton();
            }
        }
        return instance;
    }
    protected Object readResolve() { return instance; } // block serialization creating new instance
    @Override
    protected Object clone() throws CloneNotSupportedException { throw new CloneNotSupportedException(); } // block cloning
}
```

**43. Design patterns — API Gateway, Circuit Breaker, Saga (orchestration vs choreography).**

- **API Gateway** — single entry point routing to microservices. 
- **Circuit Breaker** — stops calling a failing service after threshold breaches, uses fallback. 
- **Saga** — distributed transaction pattern via local transactions + compensating events; 
- **Orchestration** = a central coordinator tells each service what to do next; 
- **Choreography** = services react to each other's events independently, no central controller. 
- **Singleton** can be broken via reflection (`setAccessible(true)`), cloning, or deserialization — each needs explicit guards (as shown in Q42).

---

## Spring Framework & Spring Boot

**44. Spring type of Dependency Injection and which is better.**

- **Constructor injection** (preferred) — dependencies are immutable (`final`), makes required dependencies explicit, and works well with unit testing. 

- **Setter injection** — optional dependencies, allows reconfiguration after construction. 

- **Field injection** (`@Autowired` on the field) — discouraged, harder to test and hides dependencies.

- ```java
  @Service
  public class OrderService {
      private final PaymentService paymentService;
      public OrderService(PaymentService paymentService) { // constructor injection — preferred
          this.paymentService = paymentService;
      }
  }
  ```

- Bean scope: default is `singleton` (one shared instance); use `prototype` for a new instance per request.


**45. @Configuration.**

```java
@Configuration
public class AppConfig {
    @Bean
    public PasswordEncoder passwordEncoder() { return new BCryptPasswordEncoder(); }
}
```

Marks a class as a source of bean definitions for the Spring IoC container.

**46. Maven dependencies/annotations, build lifecycle, package vs install.**

- Lifecycle phases run in order: `validate → compile → test → package → install → deploy`. 
- `mvn package` builds a JAR/WAR into the local `target/` folder. 
- `mvn install` does that AND copies it into the local `.m2` repository so other local projects can depend on it.

**47. Spring Boot CRUD project for an Employee Entity.**

```java
@Entity
class Employee {
    @Id @GeneratedValue private Long id;
    private String name;
    private double salary;
}

@Repository
interface EmployeeRepo extends JpaRepository<Employee, Long> {}

@RestController
@RequestMapping("/employees")
class EmployeeController {
    @Autowired private EmployeeRepo repo;

    @PostMapping public Employee create(@RequestBody Employee e) { return repo.save(e); }
    @GetMapping public List<Employee> getAll() { return repo.findAll(); }
    @GetMapping("/{id}") public Employee getOne(@PathVariable Long id) { return repo.findById(id).orElseThrow(); }
    @PutMapping("/{id}") public Employee update(@PathVariable Long id, @RequestBody Employee e) {
        e.setId(id); return repo.save(e);
    }
    @DeleteMapping("/{id}") public void delete(@PathVariable Long id) { repo.deleteById(id); }
}
```

**48. Difference between CRUD and JPA Repository.**

- `CrudRepository` provides basic operations (save, findById, delete). 
- `JpaRepository` extends it plus adds pagination, sorting, and batch operations (`flush()`, `deleteInBatch()`) — use `JpaRepository` when you need those extras.

**49. Implement pagination and sorting for /getAllEmployees.**

```java
@GetMapping
public Page<Employee> getAllEmployees(
        @RequestParam int page, @RequestParam int size, @RequestParam String sortBy) {
    return repo.findAll(PageRequest.of(page, size, Sort.by(sortBy)));
}
// GET /employees?page=0&size=10&sortBy=salary
```

**50. Global exception handling using @ControllerAdvice.**

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(EmployeeNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public Map<String, String> handleNotFound(EmployeeNotFoundException ex) {
        return Map.of("error", ex.getMessage());
    }
}
// Output: 404 { "error": "Employee not found" }
```

**51. Distributed transactions and @Transactional scopes.**
Since a single `@Transactional` only covers one local DB transaction, distributed transactions across services use the **Saga pattern** — each service commits locally and publishes an event; failures trigger compensating actions instead of a single rollback across services.

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void processPayment() { ... } // always runs in its own separate transaction
```

**52. Exception handling and propagation into inheritance.**
If a parent method declares a checked exception, an overriding subclass method **cannot throw a broader checked exception** (can throw the same, a subclass of it, or none) — this preserves the parent's contract for callers. Unchecked exceptions have no such restriction.

---

## Spring Security & JWT

**53. OncePerRequestFilter.**

```java
public class JwtFilter extends OncePerRequestFilter {
    protected void doFilterInternal(HttpServletRequest req, HttpServletResponse res, FilterChain chain) {
        // validate JWT here
        chain.doFilter(req, res);
    }
}
```

A Spring base class guaranteeing a filter executes **exactly once per request**, even in complex dispatch scenarios (like forwards/includes) where a normal filter might run multiple times.

**54. Why multiple filters?**
Each filter handles one concern — e.g., one validates JWT, another logs requests, another handles CORS. This keeps responsibilities separated (single responsibility) instead of one giant filter doing everything.

**55. Custom filter vs Spring Security filter.**
Spring Security's built-in filters (like `UsernamePasswordAuthenticationFilter`) handle standard login flows. A custom filter (like my `JwtFilter`) is added into that chain to handle something Spring doesn't provide out of the box — like validating a JWT on every request.

**56. UsernamePasswordAuthenticationFilter.**
A built-in Spring Security filter that intercepts login requests (username/password), authenticates via `AuthenticationManager`, and on success, sets the authenticated user into `SecurityContextHolder`.

**57. filterChain.doFilter()**

```java
chain.doFilter(request, response); // passes control to the NEXT filter in the chain
```

Called at the end of a filter's logic to continue the request down the filter chain — without it, the request stops there and never reaches the controller.

**58. SecurityContextHolder.**

```java
Authentication auth = SecurityContextHolder.getContext().getAuthentication();
String username = auth.getName();
```

Holds the currently authenticated user's details for the duration of the request — any part of the app can retrieve "who is logged in right now" from it.

**59. @EnableWebSecurity.**
Activates Spring Security's web security support and integrates it with Spring MVC — required to enable custom `SecurityFilterChain` configuration.

**60. SecurityFilterChain.**

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.csrf(csrf -> csrf.disable())
        .authorizeHttpRequests(auth -> auth
                               .requestMatchers("/public/**").permitAll()
                               .requestMatchers("/employees/**").authenticated()
                               .anyRequest().permitAll());
    return http.build();
}
```

Defines which URLs require authentication, which roles can access what, and how login/logout behaves.

**62. Explain JWT flow.**

1. User logs in with credentials → server verifies and generates a JWT
2. Client stores the JWT and sends it as `Authorization: Bearer <token>` on every request
3. A filter on the server validates the token's signature and expiry on each request — no server-side session needed (stateless).

**63. First step in JWT validation.**
Verify the token's **signature** using the secret/public key — if the signature doesn't match, the token was tampered with or forged, and the request is rejected immediately before even checking expiry or claims.

**64. Explain Spring Security flow.**
Request → Security Filter Chain (custom filters like JWT validation run here) → `AuthenticationManager` validates credentials → on success, user details stored in `SecurityContextHolder` → request proceeds to the controller if authorized.

**65. Full JWT with role-based mapping.**

```java
// Generate token with role claim
String token = Jwts.builder()
    .setSubject(username)
    .claim("role", "ADMIN")
    .setExpiration(new Date(System.currentTimeMillis() + 900000))
    .signWith(secretKey).compact();

// In JwtFilter — extract and set authority
Claims claims = Jwts.parserBuilder().setSigningKey(secretKey).build().parseClaimsJws(token).getBody();
String role = claims.get("role", String.class);
List<GrantedAuthority> authorities = List.of(new SimpleGrantedAuthority("ROLE_" + role));
UsernamePasswordAuthenticationToken authToken =
    new UsernamePasswordAuthenticationToken(claims.getSubject(), null, authorities);
SecurityContextHolder.getContext().setAuthentication(authToken);
```

Then use `@PreAuthorize("hasRole('ADMIN')")` on protected endpoints.

**66. doFilter, jwtFilter, security config.**
`chain.doFilter()` passes the request to the next filter. `JwtFilter` (custom, `OncePerRequestFilter`) validates the token before the request reaches the controller. `SecurityConfig` (`@Configuration` + `SecurityFilterChain`) registers the filter into the chain via `http.addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class)`.

**67. Security filter, API filter validate method.**
The filter's `doFilterInternal()` extracts the token from the `Authorization` header, verifies its signature and expiry, and if valid, sets authentication into `SecurityContextHolder` before calling `chain.doFilter()`. If invalid, it can short-circuit and return 401 without calling `doFilter()`.

---

## Microservices Architecture

**68. Why Gateway?**
Provides a single entry point for clients — centralizes routing, authentication, rate limiting, and logging instead of duplicating this logic in every microservice.

**69. Spring Cloud Gateway.**
A single entry point that routes incoming requests to the correct microservice, and can also handle cross-cutting concerns like authentication, rate limiting, and logging centrally instead of duplicating them in every service.

**70. Authentication at Gateway vs Backend.**
Gateway-level auth validates the token once for every incoming request before it even reaches a microservice — reduces duplicate validation logic across services. Backend-level auth is for fine-grained, service-specific authorization (e.g., role checks for a specific action).

**71. Why not implement JWT in every microservice?**
Duplicating JWT validation logic in every service means more code to maintain, inconsistent implementations, and more points of failure — centralizing it at the Gateway keeps it DRY and consistent.

**72. What should Gateway handle?**
Authentication (token validation), routing, rate limiting, basic request logging, and CORS.

**73. What should Backend handle?**
Authorization (fine-grained role/permission checks specific to that service's business logic), the actual business logic, and data access.

**74. RestTemplate.**

```java
User user = restTemplate.getForObject("http://user-service/users/1", User.class);
```

A synchronous HTTP client used to call other services/APIs. It's the traditional blocking option (now largely superseded by `WebClient`, but still common in existing codebases).

**75. What is Circuit Breaker?**

```java
@CircuitBreaker(name = "paymentService", fallbackMethod = "fallback")
public String callPayment() { return restTemplate.getForObject(url, String.class); }

public String fallback(Exception e) { return "Service unavailable"; }
```

Stops repeatedly calling a failing service — after enough failures, it "opens" and short-circuits calls to a fallback instead of hammering the broken service, preventing cascading failures.

**76. Retry vs Circuit Breaker.**
**Retry** = try the same failing call again (a few times), useful for transient/temporary glitches. **Circuit Breaker** = stops calling entirely after repeated failures, useful when a service is consistently down, to avoid wasting resources. Often used together — retry a few times, then circuit breaker kicks in if it keeps failing.

---

## Database & SQL

```sql
-- Sample Data
name    | department | salary
--------|------------|-------
Alice   | IT         | 90000
Bob     | IT         | 90000
Charlie | IT         | 85000
David   | HR         | 70000
Eve     | HR         | 65000
Frank   | Finance    | 60000
```

**78. What is PARTITION BY?**

- *`PARTITION BY` divides rows into groups (like department here) so window functions (RANK, ROW_NUMBER) reset and calculate independently within each group, instead of across the whole table.*

- **With** Partition By: 

  - ```sql
    SELECT name, department, salary,
           RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
    FROM employees;
    -- OUTPUT
    name    | department | salary | rnk
    --------|------------|--------|-----
    Alice   | IT         | 90000  | 1
    Bob     | IT         | 90000  | 1
    Charlie | IT         | 85000  | 3
    David   | HR         | 70000  | 1
    Eve     | HR         | 65000  | 2
    Frank   | Finance    | 60000  | 1
    ```

- **Without** Partition By: 

  - ```sql
    SELECT name, department, salary,
           RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees;
    -- OUTPUT
    name    | department | salary | rnk
    --------|------------|--------|-----
    Alice   | IT         | 90000  | 1
    Bob     | IT         | 90000  | 1
    Charlie | IT         | 85000  | 3
    David   | HR         | 70000  | 4
    Eve     | HR         | 65000  | 5
    Frank   | Finance    | 60000  | 6
    ```

- Real use case — find the top earner per department

  - ```sql
    SELECT * FROM (
        SELECT name, department, salary,
               RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
        FROM employees
    ) t
    WHERE rnk = 1;
    -- OUTPUT
    name  | department | salary | rnk
    ------|------------|--------|-----
    Alice | IT         | 90000  | 1
    Bob   | IT         | 90000  | 1
    David | HR         | 70000  | 1
    Frank | Finance    | 60000  | 1
    ```



**79. Difference between ROW_NUMBER(), RANK(), and DENSE_RANK().**

- All three assign a **sequential number** to rows based on an `ORDER BY`, but they handle **ties (equal values)** differently.

  - ```sql
    -- Sample Data employees table
    name    | salary
    --------|-------
    Alice   | 90000
    Bob     | 90000
    Charlie | 85000
    David   | 80000
    ```

  - ```sql
    -- The three functions side by side
    SELECT name, salary,
           ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num,
           RANK()       OVER (ORDER BY salary DESC) AS rank_num,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank_num
    FROM employees;
    
    -- OUTPUT
    name    | salary | row_num | rank_num | dense_rank_num
    --------|--------|---------|----------|----------------
    Alice   | 90000  | 1       | 1        | 1
    Bob     | 90000  | 2       | 1        | 1
    Charlie | 85000  | 3       | 3        | 2
    David   | 80000  | 4       | 4        | 3
    ```

- What's different

  - `ROW_NUMBER()` — always gives **unique, sequential** numbers, even for ties. It just breaks ties arbitrarily (Alice gets 1, Bob gets 2, even though they have the same salary).
  - `RANK()` — ties get the **same rank**, but it **leaves a gap** afterward. Alice and Bob both get rank 1, but the next row (Charlie) jumps to rank 3 (not 2) — because "2" is skipped to account for the 2 people tied at rank 1.
  - `DENSE_RANK()` — ties get the **same rank**, but **no gap** afterward. Alice and Bob both get rank 1, and Charlie gets rank 2 (not 3) — the next rank is always "current rank + 1", never skipping.

- Simple way to remember

- | Function       | Ties?                    | Gaps after ties?       |
  | -------------- | ------------------------ | ---------------------- |
  | `ROW_NUMBER()` | No — always unique       | N/A                    |
  | `RANK()`       | Yes — same rank for ties | Yes — skips numbers    |
  | `DENSE_RANK()` | Yes — same rank for ties | No — stays consecutive |

- **Common real use case — get the 2nd highest salary (handling ties correctly)**

  - ```sql
    SELECT name, salary FROM (
        SELECT name, salary,
               DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
        FROM employees
    ) t
    WHERE rnk = 2;
    
    -- OUTPUT
    name    | salary
    --------|-------
    Charlie | 85000
    ```

  - Using `DENSE_RANK()` here is safer than `ROW_NUMBER()` — if two people were tied for highest salary, `ROW_NUMBER()` would incorrectly treat one of them as "2nd highest," while `DENSE_RANK()` correctly treats both as "1st" and picks the true next distinct value as 2nd.

**80. Department-wise Top 3 Highest Salaries (SQL).**	

```sql
SELECT * FROM (
  SELECT name, department, salary,
         DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
  FROM employees
) t WHERE rnk <= 3;
```

**81. Highest, 2nd highest salary.**

```sql
-- Highest salary
SELECT MAX(salary) FROM employees;

-- 2nd highest salary
SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees);

-- Department-wise 2nd Highest Salaries (handles ties correctly)
SELECT name, salary FROM (
  SELECT name, salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk FROM employees
) t WHERE rnk = 2;
```

**82. SQL — grouping by, having, joins.**

```sql
SELECT department, COUNT(*) AS emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;   -- filters GROUPS, unlike WHERE which filters rows before grouping

SELECT e.name, d.name FROM employees e
JOIN departments d ON e.dept_id = d.id;  -- INNER JOIN — only matching rows from both tables
```

**83. What is Table Scan?**
The DB engine reads every row in a table sequentially to find matches — happens when there's no useful index for the query, making it slow on large tables. An "Index Seek" (using an index) is much faster.

**84. Difference between Scan and Seek.**
**Scan** = DB reads the entire table/index row by row. **Seek** = DB uses an index's tree structure to jump directly to matching rows. Seek is faster and used when a good index exists for the query's `WHERE` clause.

**85. What is an Index?**
A separate data structure (usually a B-Tree) storing sorted references to table rows for a specific column — speeds up lookups (like a book's index) at the cost of extra storage and slightly slower writes (since the index must be updated too).

**86. How do you identify a slow SQL query?**
Run `EXPLAIN ANALYZE` on the query to see the actual execution plan — check if it's doing a full table scan instead of an index seek, look at estimated vs actual row counts, and check for missing indexes on filtered/joined columns.

**87. How do you use EXPLAIN ANALYZE / Execution Plan?**

```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE customer_id = 101;
```

It shows the actual query plan the DB used — whether it did an Index Scan or Seq Scan, actual execution time per step, and row counts — helping pinpoint exactly where a query is slow.

**88. What if the database is slow?**
Check the query's execution plan for missing indexes, check for lock contention/blocking queries, review connection pool size, and consider caching (Redis) for frequently read, rarely changed data.

**89. How do you work with the DBA team?**
I share the slow query along with its execution plan, discuss whether an index change, query rewrite, or partitioning would help, and coordinate on production-safe timing for any schema changes (like adding indexes on large tables).

**90. Dynamically switching between multiple databases per request.**
Use Spring's **AbstractRoutingDataSource** — it picks the correct `DataSource` at runtime based on a key (e.g., a tenant ID from the request), set into a `ThreadLocal` before the query executes.

```java
public class TenantRoutingDataSource extends AbstractRoutingDataSource {
    protected Object determineCurrentLookupKey() {
        return TenantContext.getCurrentTenant(); // reads from ThreadLocal
    }
}
```

---

## Redis & Caching

**91. Redis.**
An in-memory key-value data store used for caching, session storage, rate limiting, and pub/sub messaging — extremely fast since data lives in memory rather than disk.

**92. Why Redis?**
It's an in-memory data store — extremely fast for caching frequently accessed data, storing sessions/tokens, or rate-limiting counters, reducing load on the primary database.

**93. Redis for JWT?**
Yes — storing JWTs (or their metadata) in Redis lets you invalidate/blacklist a token before its natural expiry (e.g., on logout), which a stateless JWT alone can't do.

**94. Will 1 lakh JWT tokens be stored in Redis?**
Yes, easily — Redis is designed for millions of key-value pairs in memory; 1 lakh (100,000) small JWT entries is a trivial load for it.

**95. How do you fetch one JWT from Redis?**

```java
String token = redisTemplate.opsForValue().get("jwt:" + userId);
```

Store with a key pattern like `jwt:<userId>`, then a direct `GET` by key — O(1) lookup.

---

## Performance & Troubleshooting

**96. API taking 15-20 seconds. How do you troubleshoot?**
Check application logs for where time is spent, use APM/tracing (like Jaeger) to see which downstream call/DB query is the bottleneck, check for N+1 query issues, thread pool exhaustion, or a slow third-party API call without a timeout.

**97. What logs will you check?**
Application logs (exceptions, timing), access logs (request/response times), DB slow query logs, and distributed tracing logs (trace ID) if it's a microservices call chain.

**98. What if a third-party API is slow?**
Add a timeout so my service doesn't hang indefinitely, wrap the call in a Circuit Breaker (Resilience4j) with a fallback, and consider caching the response if data doesn't change often.

**99. High CPU usage and slow response under heavy load — troubleshooting steps.**
Take a thread dump to find CPU-heavy or blocked threads, check for excessive garbage collection (via GC logs — could indicate memory pressure), review for inefficient loops/algorithms, check DB connection pool exhaustion, and use profiling tools (VisualVM, JFR) to pinpoint hot methods.

**100. Securing inter-service communication.**
Use **mutual TLS (mTLS)** so services verify each other's identity, or a service mesh (Istio) for automatic encryption between services. For authorization between services, use short-lived service tokens (client credentials OAuth2 flow) rather than passing user JWTs blindly downstream.

---

## System Design

**101. Healthcare project system design, Elasticsearch.**
For a healthcare system, patient records, appointments, and prescriptions each become their own microservice with their own DB (respecting HIPAA-style data isolation). **Elasticsearch** would be used for fast full-text search — e.g., searching patient records or doctor notes by keyword — since relational DBs aren't optimized for free-text search at scale.

---

## Production Support & Ownership

**102. Are you comfortable with production support?**
Yes — I've handled production bug fixes at TCS (ETQ, sync issues) and currently support the insurance platform at PwC.

**103. Have you handled production incidents?**
Yes — analyzed and resolved multiple production bugs on SBI Card's Sales24 platform, working with logs and execution traces to identify root causes quickly.

**104. Are you comfortable with late-night deployments?**
Yes — I understand production deployments sometimes need off-peak timing to minimize user impact, and I'm comfortable with that when required.

**105. Are you comfortable taking ownership of a module?**
Yes — I built the Recruitment Tracking System and the User Source Code Mapping tool end-to-end, from requirements to deployment.
