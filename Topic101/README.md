# Builder Design Pattern with Abstract Class

The only drawback in this solution is that you must make the constructor public (i.e. define it with public access modifiers).
Nevertheless, you can make it accept the builder itself as an argument.

# Solution 1

### The Abstract Class 

```Java
public abstract class Class1 {

    String str;

    /**
     * The only exist constraint is that you have to make the constructor public!
     */
    public Class1(Class1.Builder builder) {
        this.str = String.valueOf(builder.str);
    }

    /**
     * The Abstract Method
     */
    public abstract void test();

    /**
     * Get value of variable
     */
    public String getStr() {
        return str;
    }

    /**
     * The Builder
     */
    public static class Builder {

        String str;

        public Builder() {
            str = "";
        }

        public Builder withStr(String str) {
            this.str = str;
            return this;
        }

        /**
         * The build function should be like this
         */
        public Class1 build(Class<? extends Class1> c) {
            try {
                return c.getDeclaredConstructor(Class1.Builder.class).newInstance(this);
            } catch (Exception ignored) {}
            return null;
        }

    }
}
```

### The Implemented Class 

```Java
public class Class2 extends Class1 {

    /**
     * The constructor must exist
     */
    public Class2(Class1.Builder builder) {
        super(builder);
    }

    /**
     * Your implementation of the abstract method/function
     */
    @Override
    public void test() {
        System.out.println("The Implementation");
    }

}
```

### The Calling Process

```java
Class1 x = new Class1.Builder().withStr("Almazari").build(Class2.class);

System.out.println(x.getStr());
x.test();
```




# Solution 2

### The Abstract Class

```Java
public abstract class Class1 {

    String str;

    /**
     * The only exist constraint is that you have to make the constructor public!
     */
    public Class1(String str) {
        this.str = String.valueOf(str);
    }

    /**
     * The Abstract Method
     */
    public abstract void test();

    /**
     * Get value of variable
     */
    public String getStr() {
        return str;
    }

    /**
     * The Builder
     */
    public static class Builder {

        String str;

        public Builder() {
            str = "";
        }

        public Builder withStr(String str) {
            this.str = str;
            return this;
        }

        /**
         * The build function should be like this
         */
        public Class1 build(Class<? extends Class1> c) {
            try {
                return c.getDeclaredConstructor(String.class).newInstance(str);
            } catch (Exception ignored) {}
            return null;
        }

    }
}
```

### The Implemented Class

```java
public class Class2 extends Class1 {

    /**
     * The constructor must exist
     */
    public Class2(String str) {
        super(str);
    }

    /**
     * Your implementation of the abstract method/function
     */
    @Override
    public void test() {
        System.out.println("The Implementation");
    }

}
```

### The Calling Process

```java
Class1 x = new Class1.Builder().withStr("Almazari").build(Class2.class);

System.out.println(x.getStr());
x.test();
```