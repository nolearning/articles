# Stream API

## IntStream vs. Stream<Integer>
`Arrays.stream(int[])` creates an `IntStream`, not a `Stream<Integer>`

```Java
import java.util.stream.IntStream;
import java.util.Arrays;

public class HelloWorld{

     public static void main(String []args){
        // error: incompatible types: bad return type in lambda expression
       	// IntStream.of(1,2,3).map(i -> "" + i);
       	//                                ^
        //String cannot be converted to int
        // fix it should use mapToObj

        int[] arr = IntStream.of(1,2,3).map(i -> i * 2).toArray();
        System.out.println(Arrays.toString(arr));
     }
}
```

```Java
List<Integer> ints = IntStream.of(1,2,3,4,5)
                .boxed()
                .collect(Collectors.toList());
         
System.out.println(ints);
 
Output:
 
[1, 2, 3, 4, 5]
```
* [Convert IntStream to collection or array](https://howtodoinjava.com/java8/convert-intstream-collection-array/)
* [Why can't I map integers to strings when streaming from an array?](https://stackoverflow.com/questions/29028980/why-cant-i-map-integers-to-strings-when-streaming-from-an-array)
