# java-knowledge-set
Java Consumer, Supplier, Predicate \
Consumer: receive input argument, define action to that input and return nothing
```
@Test
public void whenNamesPresentUseBothConsumer(){
    List<String> cities = Arrays.asList("Sydney", "Dhaka", "New York", "London");

    Consumer<List<String>> upperCaseConsumer = list -> {
        for(int i=0; i< list.size(); i++){
            list.set(i, list.get(i).toUpperCase());
        }
    };
    Consumer<List<String>> printConsumer = list -> list.stream().forEach(System.out::println);

    upperCaseConsumer.andThen(printConsumer).accept(cities);
}
```
Note: many consumers can be chained into list of operations which should be performed on argument \
Note: biconsumer for 2 arguments at the same time \
Supplier: create something from nothing
```
@Test
public void supplier(){
    Supplier<Double> doubleSupplier1 = () -> Math.random();
    DoubleSupplier doubleSupplier2 = Math::random;

    System.out.println(doubleSupplier1.get());
    System.out.println(doubleSupplier2.getAsDouble());
}
```
Note: Supplier interface allow deferred execution, calls get() when needed \
Predicate: evaluate if element matches pre-defined condition, which returns boolean value
```
@Test
public void testPredicateAndComposition(){
    List<String> names = Arrays.asList("John", "Smith", "Samueal", "Catley", "Sie");
    Predicate<String> startPredicate = str -> str.startsWith("S");
    Predicate<String> lengthPredicate = str -> str.length() >= 5;
    names.stream().filter(startPredicate.and(lengthPredicate)).forEach(System.out::println);
}
```
Note: predicate.test() returns boolean value \
Function: receives type A -> operate -> return type B
```
@Test
public void testFunctions(){
    List<String> names = Arrays.asList("Smith", "Gourav", "Heather", "John", "Catania");
    Function<String, Integer> nameMappingFunction = String::length;
    List<Integer> nameLength = names.stream().map(nameMappingFunction).collect(Collectors.toList());
    System.out.println(nameLength);
}
```
