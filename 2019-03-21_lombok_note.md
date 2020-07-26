# Remove Spring Autowired with Lombok

Many of you are familiar with the look of this

```java
@Service
public class MyClass {
    
    @Autowired
    IEmployeeService employeeService;

    @Autowired
    IDepartmentService departmentService;
    
    @Autowired
    @Qualifier("emailService")
    INotificationService emailService;

    @Autowired
    @Qualifier("smsService")
    INotificationService smsService;

    // Business Logic
}
```

As your class grow, this could get quite long and tedious to repeatedly adding "@Autowired" too all the fields.

Not count the fact that your IntelliJ could be spamming you with warning that you are using "reflection" for dependency injection, which is slow and dangerous.

So lets try to fix this

```java
@Service
public class MyClass {
    
    private final IEmployeeService employeeService;
    private final IDepartmentService departmentService;
    private final INotificationService emailService;
    private final INotificationService smsService;

    @Autowired
    public MyClass(IEmployeeService employeeService,
                   IDepartmentService departmentService,
                   @Qualifier("emailService") INotificationService emailService;
                   @Qualifier("smsService") INotificationService smsService;
    ) {
        // Assert Not null
        Assert.notNull(employeeService);
        ...

        // Assigned to fields
        this.employeeService = employeeService;
        ...
    }

    // Business Logic
}
```

Did you know that from Spring 4.3, if your class has only a single constructor, Spring will automatically use it for autowiring without the need of "@Autowired" ?

Let's just remove that annotation then.

```java
@Service
public class MyClass {
    
    private final IEmployeeService employeeService;
    private final IDepartmentService departmentService;
    private final INotificationService emailService;
    private final INotificationService smsService;

    // No need Autowired annotation here thanks to Spring 4.3
    public MyClass(IEmployeeService employeeService,
                   IDepartmentService departmentService,
                   @Qualifier("emailService") INotificationService emailService;
                   @Qualifier("smsService") INotificationService smsService;
    ) {
        // Assert Not null
        Assert.notNull(employeeService);
        ...

        // Assigned to fields
        this.employeeService = employeeService;
        ...
    }

    // Business Logic
}
```

Q: But Son, null assertion is bulky, and I am lazy!!!

A: No worry i gotchu fam! Introducing: [Lombok @NonNull](https://projectlombok.org/features/NonNull)

```java
@Service
public class MyClass {
    
    private final IEmployeeService employeeService;
    private final IDepartmentService departmentService;
    private final INotificationService emailService;
    private final INotificationService smsService;

    // No need Autowired annotation here thanks to Spring 4.3
    public MyClass(@NonNull IEmployeeService employeeService,
                   @NonNull IDepartmentService departmentService,
                   @NonNull @Qualifier("emailService") INotificationService emailService;
                   @NonNull @Qualifier("smsService") INotificationService smsService;
    ) {
        // Assigned to fields
        this.employeeService = employeeService;
        ...
    }

    // Business Logic
}
```

Hmm this looks oddly familiar isnt it?

You must have seen this somewhere before

That's right, its quite similar to a POJO class where you have replaced your constructor using Lombok!

Let us do just that.

```java
@Service
@RequiredArgsConstructor
public class MyClass {
    
    private final IEmployeeService employeeService;
    private final IDepartmentService departmentService;
    private final INotificationService emailService;
    private final INotificationService smsService;

    // Business Logic
}
```

Q: But hey, where are my null check?

A: Fine, we can just move that to the parameters

```java
@Service
@RequiredArgsConstructor
public class MyClass {
    
    @NonNull private final IEmployeeService employeeService;
    @NonNull private final IDepartmentService departmentService;
    @NonNull private final INotificationService emailService;
    @NonNull private final INotificationService smsService;

    // Business Logic
}
```

Q: Wait a minute... now I have 2 `INotificationService` implementation to autowire, that would be a conflict without `@Qualifier`

A: Good eyes, previously this is a hard problem to solve, but look at this magic since lombok version `1.18.4`

```
# lombok.config 
# this file should be placed at project's root directory

# Copy the Qualifier annotation from the instance variables to the constructor
# see https://github.com/rzwitserloot/lombok/issues/745
lombok.copyableAnnotations += org.springframework.beans.factory.annotation.Qualifier

# this is to annotate all lombok generated code with @Generated
# used for ignore code coverage in testing
lombok.addLombokGeneratedAnnotation = true
```

```java
@Service
@RequiredArgsConstructor
public class MyClass {
    
    @NonNull private final IEmployeeService employeeService;
    @NonNull private final IDepartmentService departmentService;
    @NonNull @Qualifier("emailService") private final INotificationService emailService;
    @NonNull @Qualifier("smsService")   private final INotificationService smsService;

    // Business Logic
}
```

BAM! Now your code is a lot cleaner thanks to the magic of Lombok

