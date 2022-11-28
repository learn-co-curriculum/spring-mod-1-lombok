# Lombok Dependency

## Learning Goals

- Show how to add the Lombok dependency to a Maven project.
- Discuss the advantages of the dependency.

## Introduction

As we saw in the last lesson, the DTO design pattern is a simple POJO that only
consists of accessor and mutator methods. It doesn't define any additional
business logic. While we noticed in earlier lessons, we can actually have IntelliJ
create these getter and setter methods for us since it is so common to have these
methods!

To help write DTOs, there's a dependency we can import to help cut down the amount
of code we write! This is where the Lombok dependency comes in! We'll first
discuss adding it to our Maven project, and then we'll cover the basis of how
to take advantage of this dependency to reduce the lines of code we have to write!

## Adding the Lombok Dependency

We can either:

1. Add the dependency to an existing Maven project with a pom.xml.
2. Create a new Spring Boot application using the Spring Initializr

We'll cover both options in this lesson in case you want to add a dependency to
an existing project or create a new one!

### Adding Lombok to an Existing Maven Project

1. Open up the pom.xml in the Maven project.
2. Under the `<dependencies>` header, add the following section to add the Lombok
   dependency:

```xml
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.24</version>
            <scope>provided</scope>
        </dependency>
```

Note: the version specified in this lesson may differ from what is the most
up-to-date version. Check out [this page here](https://projectlombok.org/changelog)
to check the latest version.

3. Reload the Maven project to finish adding the dependency.

For most of these lessons, we have chosen to use Maven as a build tool. But if
using Gradle instead, check out this page here to see how to add the Lombok
dependency: [Adding Lombok to a Gradle Project](https://projectlombok.org/setup/gradle).

### Adding Lombok to a New Spring Project

1. Navigate to the [Spring Initializr](https://start.spring.io/).
2. Select the project properties.
    1. Select "Maven Project", as we will use Maven as the build tool.
    2. Select "Java" as the language.
    3. Select the most recent release version of Spring Boot 2. (Make sure it does
       not have "SNAPSHOT" listed after it.)
    4. Select the appropriate Java JDK version.
3. Add dependencies.
   1. Click "ADD DEPENDENCIES".
   2. Search for "lombok".
   3. Select "Lombok" from the list.
   4. Add any other dependencies by repeating this process.
4. Click on the “Generate” button on the bottom. This will download a zip file
   containing the Spring Boot project.
5. Unzip the archive and open it in a preferred code editor or IDE.

![Spring Initializr Lombok Dependency](https://curriculum-content.s3.amazonaws.com/spring-mod-1/lombok/spring-initializr-lombok.png)

## Advantages of the Lombok Dependency

The Lombok dependency can help minimize and reduce the amount of boilerplate code.
When we refer to "boilerplate code", we are referring to code that may be repeated
in various sections of the code with little to no variation. Prime suspects would
be constructors that don't really do anything and getters and setters.

Let's review the `FootballTeamDTO` class again:

```java
package com.example.springwebdemo.dto;

public class FootballTeamDTO {
    private String teamName;
    private int wins;
    private int losses;
    private boolean currentSuperBowlChampion;

    public String getTeamName() {
        return teamName;
    }

    public void setTeamName(String teamName) {
        this.teamName = teamName;
    }

    public int getWins() {
        return wins;
    }

    public void setWins(int wins) {
        this.wins = wins;
    }

    public int getLosses() {
        return losses;
    }

    public void setLosses(int losses) {
        this.losses = losses;
    }

    public boolean getCurrentSuperBowlChampion() {
        return currentSuperBowlChampion;
    }

    public void setCurrentSuperBowlChampion(boolean currentSuperBowlChampion) {
        this.currentSuperBowlChampion = currentSuperBowlChampion;
    }
}
```

### Constructors

If we wanted to add some constructors to this class, normally we would add code
that may look like this:

```java

// Empty constructor
public FootballTeamDTO() {}

// Constructor to set every field
public FootballTeamDTO(String teamName, int wins, int losses, boolean currentSuperBowlChampion) {
    this.teamName = teamName;
    this.wins = wins;
    this.losses = losses;
    this.currentSuperBowlChampion = currentSuperBowlChampion;
}
```

Since this type of pattern in creating the constructor is usually common across
most classes, we could actually use an annotation part of the Lombok package to
reduce the amount of code we have to write:

```java
import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;

@NoArgsConstructor
@AllArgsConstructor
public class FootballTeamDTO {
   private String teamName;
   private int wins;
   private int losses;
   private boolean currentSuperBowlChampion;

   // no constructors

}
```

The `@NoArgsConstructor` annotation is used here to generate a default constructor
without any arguments. This annotation replaces the code:

```java
public FootballTeamDTO() {}
```

What happens is when we compile the code, the Lombok annotation will auto-generate
this constructor into the .class file. This will hold true for the other Lombok
annotations that we add.

The `@AllArgsConstructor` annotation is used to create a constructor with all the
fields as an argument. In this case, we have the fields `teamName`, `wins`,
`losses`, `currentSuperBowlChampion` in our `FootballTeamDTO` class. The
`@AllArgsConstructor` will create a constructor that will set each of these
instance variables and will replace this code:

```java
public FootballTeamDTO(String teamName, int wins, int losses, boolean currentSuperBowlChampion) {
    this.teamName = teamName;
    this.wins = wins;
    this.losses = losses;
    this.currentSuperBowlChampion = currentSuperBowlChampion;
}
```

### Getters and Setters

We can also use the Lombok dependency to create our getters and setters for us!
In the above code, notice how each field has a getter and setter method associated
with it. Let's look specifically at the field `teamName` for a moment to
demonstrate how the `@Getter` and `@Setter` annotations work:

Consider the following:

```java
import lombok.Getter;
import lombok.Setter;

public class FootballTeamDTO {
   @Getter
   @Setter
   private String teamName;
   private int wins;
   private int losses;
   private boolean currentSuperBowlChampion;

   // Getters and Setters for wins, losses, and currentSuperBowlChampion fields
}
```

When we put the annotation `@Getter` before defining the instance variable, it
will look at that field and create an accessor method for it. This will replace
the code:

```java
public String getTeamName() {
    return teamName;
}
```

When we add the annotation `@Setter` at the field level it will generate a
mutator method for that field, replacing the code:

```java
public void setTeamName(String teamName) {
    this.teamName = teamName;
}
```

As we mentioned above, these getter and setter methods will be auto-generated at
compile time.

Now what if we want to add a getter and setter method to each of the fields? We
could add these annotations to each of the field level, as such:

```java
import lombok.Getter;
import lombok.Setter;

public class FootballTeamDTO {
   @Getter
   @Setter
   private String teamName;
   
   @Getter
   @Setter
   private int wins;
   
   @Getter
   @Setter
   private int losses;
   
   @Getter
   @Setter
   private boolean currentSuperBowlChampion;
}
```

But an easier way to accomplish the same thing is to add the `@Getter` and
`@Setter` annotations to the class level instead of at the field level:

```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class FootballTeamDTO {
   private String teamName;
   private int wins;
   private int losses;
   private boolean currentSuperBowlChampion;
}
```

By placing the `@Getter` and `@Setter` annotations at the class level, we are
telling Lombok to add auto-generate accessor and mutator methods for each of the
instance variables defined in the class. It is the same as adding the annotations
to each of the fields and replaces this code:

```java
    public String getTeamName() {
        return teamName;
    }

    public void setTeamName(String teamName) {
        this.teamName = teamName;
    }

    public int getWins() {
        return wins;
    }

    public void setWins(int wins) {
        this.wins = wins;
    }

    public int getLosses() {
        return losses;
    }

    public void setLosses(int losses) {
        this.losses = losses;
    }

    public boolean getCurrentSuperBowlChampion() {
        return currentSuperBowlChampion;
    }

    public void setCurrentSuperBowlChampion(boolean currentSuperBowlChampion) {
        this.currentSuperBowlChampion = currentSuperBowlChampion;
    }
```

The advantage to putting the annotation at the field level is if a field
_should not_ have a getter or setter method associated with it. But if all fields
are to have a getter and setter method, then it is more advantageous to move the
annotations to the class level.

### The Data Annotation

Another annotation we will use quite often is the `@Data` annotation. This
annotation in the Lombok package actually combines the following annotations into
one:

- `@Getter`
- `@Setter`
- `@ToString`
  - For more information on this annotation, please see the documentation
    [here](https://projectlombok.org/features/ToString).
- `@EqualsAndHashCode`
  - For more information on this annotation, please see the documentation
     [here](https://projectlombok.org/features/EqualsAndHashCode).
- `@RequiredArgsConstructor`
  - For more information on this annotation, please see the documentation
    [here](https://projectlombok.org/features/constructor).

Consider the `FootballTeamDTO` class below again:

```java
import lombok.Data;

@Data
public class FootballTeamDTO {
    private String teamName;
    private int wins;
    private int losses;
    private boolean currentSuperBowlChampion;
}
```

Notice that is all the code we need to add when we make use of the `@Data`
annotation! The `@Data` annotation will generate the boilerplate code that is
normally included in DTOs and beans, like the `FootballTeamDTO` class. It will
auto-generate getter and setter methods for each field, an equals and hashCode
method given the fields, a `toString` method, and a constructor that
initializes all the final fields and all the non-final fields that should not
be set to `null`.

When we use the `@Data` method, we no longer have to include the following code:

```java
/* Will generate an empty constructor since none of the fields
   are marked final or non-null */
public FootballTeamDTO() { }

public String getTeamName() {
        return this.teamName;
        }

public int getWins() {
        return this.wins;
        }

public int getLosses() {
        return this.losses;
        }

public boolean isCurrentSuperBowlChampion() {
        return this.currentSuperBowlChampion;
        }

public void setTeamName(final String teamName) {
        this.teamName = teamName;
        }

public void setWins(final int wins) {
        this.wins = wins;
        }

public void setLosses(final int losses) {
        this.losses = losses;
        }

public void setCurrentSuperBowlChampion(final boolean currentSuperBowlChampion) {
        this.currentSuperBowlChampion = currentSuperBowlChampion;
        }

public boolean equals(final Object o) {
        if (o == this) {
        return true;
        } else if (!(o instanceof FootballTeamDTO)) {
        return false;
        } else {
        FootballTeamDTO other = (FootballTeamDTO)o;
        if (!other.canEqual(this)) {
        return false;
        } else if (this.getWins() != other.getWins()) {
        return false;
        } else if (this.getLosses() != other.getLosses()) {
        return false;
        } else if (this.isCurrentSuperBowlChampion() != other.isCurrentSuperBowlChampion()) {
        return false;
        } else {
        Object this$teamName = this.getTeamName();
        Object other$teamName = other.getTeamName();
        if (this$teamName == null) {
        if (other$teamName != null) {
        return false;
        }
        } else if (!this$teamName.equals(other$teamName)) {
        return false;
        }

        return true;
        }
        }
        }

protected boolean canEqual(final Object other) {
        return other instanceof FootballTeamDTO;
        }

public int hashCode() {
        int PRIME = true;
        int result = 1;
        result = result * 59 + this.getWins();
        result = result * 59 + this.getLosses();
        result = result * 59 + (this.isCurrentSuperBowlChampion() ? 79 : 97);
        Object $teamName = this.getTeamName();
        result = result * 59 + ($teamName == null ? 43 : $teamName.hashCode());
        return result;
        }

public String toString() {
        String var10000 = this.getTeamName();
        return "FootballTeamDTO(teamName=" + var10000 + ", wins=" + this.getWins() + ", losses=" + this.getLosses() + ", currentSuperBowlChampion=" + this.isCurrentSuperBowlChampion() + ")";
        }
```

Note: The code above is what is auto-generated in the decompiled
`FootballTeamDTO.class` file.

### More Lombok Annotations

To learn about more annotations not discussed in this lesson, please see the
[Lombok features documentation](https://projectlombok.org/features/).

## Conclusion

As we have seen in this lesson, the Lombok dependency is a powerful tool that
Java developers can take advantage of to cut down boilerplate code. This can be
used to not only create cleaner code and reduce lines of source code, but also
save time when it comes to developing.

## Resources

- [Project Lombok Changelog](https://projectlombok.org/changelog)
- [Lombok Features Documentation](https://projectlombok.org/features/)
- [SpringBoot with Lombok](https://springframework.guru/spring-boot-with-lombok-part-1/)
