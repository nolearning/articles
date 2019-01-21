# Random And Probability

## Generate a random number from existing

* [How to generate a random number between 1 and 10 with a six-sided die?](https://math.stackexchange.com/questions/1314460/how-to-generate-a-random-number-between-1-and-10-with-a-six-sided-die/1314479#1314479)
```
Roll a die
1 -> A(0, 1)
2 -> B(1, 2, 3)
3 -> A(4, 3)
4 -> A(5, 6)
5 -> B(6, 7, 8)
6 -> A(9, 8)

A(x,y): Roll a die
1, 2, 3 -> x
4 -> A(x, y)
5, 6 -> y
B(x,y,z): Roll a die
1 -> x
2 -> c(x, y)
3, 4 -> y
5 -> C(z, y)
6 -> z
C(x,y): Roll a die
1 -> x
2 -> C(x, y)
3, 4, 5, 6 -> y

A(x,y) returns x with probability 35 and y with probability 25
B(x,y,z) returns x with probability 15, y with probability 35, and z with probability 15
C(x,y) returns x with probability 15 and y with probability 45
Overall, it produces the 10 outcomes each with 110 probability. 
```

* [Expand a random range from 1–5 to 1–7](https://stackoverflow.com/questions/137783/expand-a-random-range-from-1-5-to-1-7)
``` c
int rand7()
{
    int vals[5][5] = {
        { 1, 2, 3, 4, 5 },
        { 6, 7, 1, 2, 3 },
        { 4, 5, 6, 7, 1 },
        { 2, 3, 4, 5, 6 },
        { 7, 0, 0, 0, 0 }
    };

    int result = 0;
    while (result == 0)
    {
        int i = rand5();
        int j = rand5();
        result = vals[i-1][j-1];
    }
    return result;
}

// another solution
int i;
do
{
  i = 5 * (rand5() - 1) + rand5();  // i is now uniformly random between 1 and 25
} while(i > 21);
// i is now uniformly random between 1 and 21
return i % 7 + 1;  // result is now uniformly random between 1 and 7
```



## selection of numbers from a set with equal probability

* [selection of numbers from a set with equal probability](https://stackoverflow.com/questions/8516664/selection-of-numbers-from-a-set-with-equal-probability/10449932)
```
for elem in S
  if random() < (k - S'.size)/S.size // This is float division
    S'.add(elem)vvv
```

![Selection Equation](https://scontent-sin6-2.xx.fbcdn.net/v/t1.0-9/10593095_677984275648191_2263092542368829345_n.jpg?_nc_cat=108&_nc_ht=scontent-sin6-2.xx&oh=8419bf7bdb11ba83c4fba7f55c993da7&oe=5CB649B7)
