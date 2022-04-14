## Java basic data structures: give brief description (usage and difference) on ArrayList, HashMap and HashSet

<br>

**All of them only allow non-primitive dataTypes.**
1. **ArrayList**
    * It is implemented on arrays and dymanic by size.
    * Allows null and can add duplicates.
    * It is an implementation of Collections Interface, had add(), get(), remove() major methods.
    * Maintains Inserion Order, so we can access by index.
        ```java
            List<String> values = new ArrayList<>();
            values.add("T1");
            values.add("T1");
            values.add("T2");
            values.add(null);
            values.remove("T2");
            /**
                Output : ["T1","T2",null, "T2"]
            **/
        ```
2. **HashMap**
    * Key-value datastructure
    * Does not store duplicate key, replaces exisitng value if trying to add same key.
    * Allows null as Key.
    * HashMap internal implementation works on concept of Key Hashing.
      * While put(), based on the hashCode() of key, calculate an index and store accodingly.
      * While get(), calculate the index by key and get the index. Then get the value by key.

        ```java
            Map<Integer, String> values = new HashMap<>();
            values.put(1, "T1");
            values.put(1,"T2");
            values.put(null, "T3");
            /**
                Output : {null=T3, 1=T2}
            **/
        ```
3. **HashSet**
    * Does not store duplicates
    * null can be added.
    * No insertion order manintained, and no index accessing.
        ```java
            Set<String> values = new HashSet<>();
            values.add("T1");
            values.add("T1");
            values.add("T2");
            values.add(null);
            /**
                Output : [null,"T1","T2"]
            **/
        ```
<br>

## Java8+ features (name and code snippet, e.g. Optional, stream api, lambda expression) (at least 1-2 examples)

<br>

1. **Optional**
   * To handle NullPointerException, we use Optional.
        ```java
            Map<Integer, String> values = new HashMap<>();
            Optional<String> val = Optional.ofNullable(values.get(10));
            val.ifPresent(System.out::println); // No output, nothing to print
            String test = Optional.ofNullable(values.get(10)).orElse("Should print this Line."); // Output :: Should print this Line.
        ```
2. **Stream API**
   * Streams helps in access updating/filtering/searching on collections.
   * Introduced in java-8 for functional access.
   * There are many menthods in Stream, added minor example.
        ```java
            List<String> values = new ArrayList<>();
            values.add("Event-1");
            values.add("Event-2");
            values.add("Event-3");
            List<String> series = values.stream().map(v -> "Streaming :" + v).collect(Collectors.toList());
            System.out.println(series);
            List<String> parallel = values.parallelStream().map(v -> "Parallel Streaming :" + v).collect(Collectors.toList());
            System.out.println(parallel);
            boolean containsTwo = parallel.stream().anyMatch(s -> s.contains("2"));
            System.out.println(containsTwo);
            /*  Output :
                [Streaming :Event-1, Streaming :Event-2, Streaming :Event-3]
                [Parallel Streaming :Event-1, Parallel Streaming :Event-2, Parallel Streaming :Event-3]
                true
            */
        ```
3. **Lambda Expression**
    * An Fucntional Interface with Runnable without method declaration.
        ```java
                List<String> values = new ArrayList<>();
                values.add("Event-1");
                values.add("Event-2");
                Runnable lambda1 = () -> values.forEach(System.out::println);
                List<String> valuesLower = new ArrayList<>();
                Runnable lambda2 = () -> values.stream().map(String::toLowerCase).collect(Collectors.toList()).forEach(System.out::println);
                lambda1.run();
                lambda2.run();
                /*  Output :
                        Event-1
                        Event-2
                        event-1
                        event-2
                */
        ```
<br>

## Spring(boot) annotations and brief description (at least 2-3 examples)

<br>

1. **@RestController**
   * Used to declare the entry point of RestAPI's
   * It is combination of @Controller + @ResponseBody.
   * By defining @ResponseBody, no need to declare on each API declaration in the class, response converts to application/json by default
        ```java
            @Controller
            @ResponseBody
            public @interface RestController {
                //
            }
        ```
2. **@RequestMapping**
   * 2 Use cases
   * Can be declared on class level along with @RestController and create a prefix handler for all Mappings in the class
   * Can be declared as handler for each individual endpoint.
   * There is a RequestMapping wrapper for each HTTP method like @GetMapping == @RequestMapping(method = RequestMethod.GET)
        ```java
            @RestContoller
            @RequestMapping("/sample")
            public class TestController {

                @RequestMapping("first", method = RequestMethod.GET)
                public String test() {

                }
            }
        ```
3. **@Repository**
   * Class with @Repository represents as Data Access Layer (CRUD Operations over database).
   * Its a representation of @Component with data access usages only.
        ```java
            @Repository
            public class CustomerRepo {

                findById(Integer id);
            }
        ```

<br>

## SQL: what is an inner join vs outer join? brief description

<br>

1. **Inner Join**
    * If we do INNER JOIN on two tables, should return only the matching data connected by the join column.
        ```sql
            -- This query should return only Students who enrolled in Math Subject. 
            select s.name,e.subject_name from STUDENT s 
            JOIN SUBJECTS_ENROLLED e on s.id=e.student_id
            WHERE e.subject_name='Math';
        ```
2. **Outer Join**
    * LEFT JOIN
      * On LEFT JOIN, should return only left table rows and matching rows on right table.
        ```sql
            select s.name,e.subject_name from STUDENT s 
            LEFT JOIN SUBJECTS_ENROLLED e 
            on (s.id=e.student_id and e.subject_name='Math');
        ```
    * RIGHT JOIN
      * On RIGHT JOIN, should return only right table rows and matching rows on left table.
        ```sql
            select s.name,e.subject_name from STUDENT s 
            RIGHT JOIN SUBJECTS_ENROLLED e 
            on (s.id=e.student_id and e.subject_name='Math');
        ```
    * FULL JOIN
      * On FULL JOIN, it should return all records of left and right rows combined.
      * It is an UNION of LEFT JOIN and RIGHT JOIN.
        ```sql
            select s.name,e.subject_name from STUDENT s 
            LEFT JOIN SUBJECTS_ENROLLED e 
            on (s.id=e.student_id and e.subject_name='Math')
            UNION
            select s.name,e.subject_name from STUDENT s 
            RIGHT JOIN SUBJECTS_ENROLLED e 
            on (s.id=e.student_id and e.subject_name='Math');
        ```
<br>

## design patterns: name and usage (at least 1-2 examples)

<br>

1. Singleton
   * Used to declare the class instance only once in life-cycle.
   * Mainly used for database connections, configurations.
   * In Spring-boot by default scope is singleton.
  
        ```java
            /* 
                Single Instance of class only initialize if null.
            */
            public class Test {
                private static Test instance = null;

                public static getInstance() {
                    if(instance == null) instance = new Test();
                    return instance;
                } 
            }
        ```
2. DAO Pattern
   * Used for creating data accessing API's
   * It contains an Interface with operations on data with returns a POJO and has an Implementation for the Interface.

        ```java
            /* 
                Single Instance of class only initialize if null.
            */
            public class Employee {
                private Integer id;
                private String name;
            }

            public interface EmployeeDAO {
                Employee getById(Integer id);
                List<Employee> getAll();
            }
            public class EmployeeDAOImpl implements EmployeeDAO {
                Employee getById(Integer id) {
                    // IMPL
                    return employee;
                }
                List<Employee> getAll() {
                    // IMPL
                    return employees
                }
            }
            public class Runner {
                public static void main(String args...) {
                    EmployeeDAO dao = new EmployeeDAOImpl();
                    dao.getById(1);
                }
            }
        ```
<br>
