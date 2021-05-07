
## Java Consumer, Supplier, Predicate, Function, Map and Flatmap
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
Note: functions can be chained by "compose" and can be called independently using function.apply()
```
Function<Integer, String> intToString = Object::toString; // toString an integer
Function<String, String> quote = s -> "'" + s + "'"; // append the string with quotes

Function<Integer, String> quoteIntToString = quote.compose(intToString);
assertEquals("'5'", quoteIntToString.apply(5)); // the chained function applies to number 5

```
Map: transform data in a stream like uppercase for type string 
```
List<String> myList = Stream.of("a", "b")
  .map(String::toUpperCase)
  .collect(Collectors.toList());
assertEquals(asList("A", "B"), myList);
```
Note: map may change the datatype and this depends on the function passed into it. E.g: String::toUpperCase will not change but Function<Integer, String> intToString = Object::toString will \
Flatmap: to avoid nested collections when operating on stream
```
List<Integer> list1 = Arrays.asList(1,2,3);
List<Integer> list2 = Arrays.asList(4,5,6);
List<Integer> list3 = Arrays.asList(7,8,9);
          
List<List<Integer>> listOfLists = Arrays.asList(list1, list2, list3);         
List<Integer> listOfAllIntegers = 
listOfLists.stream()
.flatMap(x -> x.stream())
.collect(Collectors.toList());

String[][] dataArray = new String[][]{{"a", "b"}, {"c", "d"}, {"e", "f"}, {"g", "h"}};
List<String> listOfAllChars = Arrays.stream(dataArray)
.flatMap(x -> Arrays.stream(x))
.collect(Collectors.toList());
```

