---
category : short
layout: post
tagline: "Mmmmmmmmm more Euler."
---

Euler problems solved using a functional Java library I created called [jFunc](https://github.com/drjoliv/jfunc).
<!--excerpt-->

* [Euler 06](#euler-06)
* [Euler 07](#euler-07)
* [Euler 08](#euler-08)
* [Euler 09](#euler-09)
* [Euler 10](#euler-10)

<hr/>

## Euler 06

> The sum of the squares of the first ten natural numbers is,
>
> `12 + 22 + ... + 102 = 385`
>
> The square of the sum of the first ten natural numbers is,
>
> `(1 + 2 + ... + 10)2 = 552 = 3025`
>
> Hence the difference between the sum of the squares of the first ten natural numbers and the square of the sum is `3025 − 385 = 2640`. Find the difference between the sum of the squares of the first one hundred natural numbers and the square of the sum.

```java
    F1<FList<Long>, Long> sqrs = F1.compose(Longs::sum, map(i -> i * i));
    F1<FList<Long>, Long> sums = F1.compose(i -> i * i, Longs::sum);
    F1<FList<Long>, Long> go   = l -> sums.call(l) - sqrs.call(l);
    return go.call(range(1, 100));
```

This problem is broken down into three parts: finding the sum of the squares, finding the square of the sums and finding their difference. Since there are three distinct pieces of the problem it is very natural to create three distinct functions(`sqrs`, `sums`, `go`).

`sqrs` and `sums` can be created by composing other functions.

[F1&lt;A,C&gt; F1.compose(F1&lt;B,C&gt; f2, F1&lt;A,B&gt; f1)](https://drjoliv.github.io/jfunc/drjoliv/jfunc/function/F1.html#compose-drjoliv.jfunc.function.F1-drjoliv.jfunc.function.F1-) composes functions, if we observe the types closely we can see that the functions given to `F1.compose` are composed from right to left.

[Longs::sum](https://drjoliv.github.io/jfunc/drjoliv/jfunc/nums/Longs.html#sum-drjoliv.jfunc.data.list.FList-) is a function of arity two that sums two Long values.

[F1&lt;FList&lt;A&gt;, FList&lt;B&gt;&gt; map(F&lt;A,B&gt; f)](https://drjoliv.github.io/jfunc/drjoliv/jfunc/data/list/Functions.html#map-drjoliv.jfunc.function.F1-) takes a function from `A -> B` and returns a function from `FList<A> -> FList<B>`.

And `go` simply combines the two functions `sqrs` and `sums` into one neat package.

[Complete Code](https://github.com/drjoliv/ProjectEulerSolutions/blob/master/src/main/java/drjoliv/euler/Euler06.java) |
<a type="button" data-toggle="collapse" data-target="#euler06" aria-expanded="false" aria-controls="euler06">
Answer
</a>
<div class="collapse" id="euler06">
  <div class="well well-sm">25164150</div>
</div>

<hr/>


## Euler 07

> By listing the first six prime numbers: 2, 3, 5, 7, 11, and 13, we can see that the 6th prime is 13. What is the 10 001st prime number?

```java
    return Numbers.primes.get(10000);
```

jFunc defines an infinite list of primes, [Numbers.primes](https://drjoliv.github.io/jfunc/drjoliv/jfunc/nums/Numbers.html#primes). Because jFuncs list type is 0-based indexed, the number 10000 is used instead of 10001.

[Complete Code](https://github.com/drjoliv/ProjectEulerSolutions/blob/master/src/main/java/drjoliv/euler/Euler07.java) |
<a type="button" data-toggle="collapse" data-target="#euler07" aria-expanded="false" aria-controls="euler07">
Answer
</a>
<div class="collapse" id="euler07">
  <div class="well well-sm">104743</div>
</div>

<hr/>

## Euler 08

> The four adjacent digits in the 1000-digit number that have the greatest product are 9 × 9 × 8 × 9 = 5832.
>
> 73167176531330624919225119674426574742355349194934
> 96983520312774506326239578318016984801869478851843
> 85861560789112949495459501737958331952853208805511
> 12540698747158523863050715693290963295227443043557
> 66896648950445244523161731856403098711121722383113
> 62229893423380308135336276614282806444486645238749
> 30358907296290491560440772390713810515859307960866
> 70172427121883998797908792274921901699720888093776
> 65727333001053367881220235421809751254540594752243
> 52584907711670556013604839586446706324415722155397
> 53697817977846174064955149290862569321978468622482
> 83972241375657056057490261407972968652414535100474
> 82166370484403199890008895243450658541227588666881
> 16427171479924442928230863465674813919123162824586
> 17866458359124566529476545682848912883142607690042
> 24219022671055626321111109370544217506941658960408
> 07198403850962455444362981230987879927244284909188
> 84580156166097919133875499200524063689912560717606
> 05886116467109405077541002256983155200055935729725
> 71636269561882670428252483600823257530420752963450
>
> Find the thirteen adjacent digits in the 1000-digit number that have the greatest product. What is the value of this product?

```java
    F1<String,Long> go = showStr() // convert String to FList<Character>
        // Converts each Character into an Integer
      .then(fc  -> fc.map(c -> Character.getNumericValue(c)))
        // Converts each Integer to Long
      .then(fi  -> fi.map(Integer::longValue))
        // A sliding window that creates a FList<FList<Long>>
      .then(fl  -> fl.window(13))
        // Converts each FList<Long> to a product.
      .then(ffl -> ffl.map(Longs::product))
        // Find the maximum product within the FList.
      .then(f   -> f.maximum(Long::compare));

    Try<Long> digits = Try.with(() -> Euler08.class.getResource("euler08.txt").openStream())
      .bind(IO.readFileFromStream)
      .map(go);
    
    return stream.get();
```

If we break the problem into a series of steps we end up with:

1. Obtain 1000 digit String from file.
2. Convert String into a FList of Characters.
3. Convert each Character into an Integer
4. Convert each Integer into a Long
5. Convert FList<Long> to FList<FList<Long>> using a sliding window to break the FList of longs into smaller FLists of size 13
6. Find the product of each FList.
7. Find the largest Long within the FList.

The Function `go` is a composition of the steps from 2 to 7.

Step 1. Is accomplished with a [Try](https://drjoliv.github.io/jfunc/drjoliv/jfunc/contorl/Try.html). A Try is used because reading the 1000 digits from a file throws a checked Exception.

[Complete Code](https://github.com/drjoliv/ProjectEulerSolutions/blob/master/src/main/java/drjoliv/euler/Euler08.java) |
<a type="button" data-toggle="collapse" data-target="#euler08" aria-expanded="false" aria-controls="euler08">
Answer
</a>
<div class="collapse" id="euler08">
  <div class="well well-sm">23514624000</div>
</div>

<hr/>

## Euler 09

> A Pythagorean triplet is a set of three natural numbers, a < b < c, for which,
>
> a^2 + b^2 = c^2
> For example, 32 + 42 = 9 + 16 = 25 = 52.
> There exists exactly one Pythagorean triplet for which a + b + c = 1000.
> Find the product abc.

```java
    FList<Integer> list = 
      start(1)
      .For( a        -> range(a + 1, 1000)
            // After genertating a and b, c is limited to only one possiblilty.
          ,(a, b)    -> flist(1000 - b - a)
          ,(a, b, c) -> guard(a*a + b*b == c*c)
          .semi(flist(a*b*c)));
    return list.head();
```

The [For](https://drjoliv.github.io/jfunc/drjoliv/jfunc/data/list/FList.html#For-drjoliv.jfunc.function.F1-drjoliv.jfunc.function.F2-) method sequences monadic operations and is analogous to a do-block in Haskell.

`(a, b)    -> flist(1000 - b - a)` after genertating a and b, c is limited to only one possiblilty.

`(a, b, c) -> guard(a*a + b*b == c*c)` filters any value from the FList where the statement `a*a + b*b == c*c` is false.

Because there exits only one Pythagorean triplet where `a + b + c = 1000` we only have to return the head of the generated list.

[Complete Code](https://github.com/drjoliv/ProjectEulerSolutions/blob/master/src/main/java/drjoliv/euler/Euler09.java) |
<a type="button" data-toggle="collapse" data-target="#euler09" aria-expanded="false" aria-controls="euler09">
Answer
</a>
<div class="collapse" id="euler09">
  <div class="well well-sm">31875000</div>
</div>

<hr/>

## Euler 10

> The sum of the primes below 10 is 2 + 3 + 5 + 7 = 17. Find the sum of all the primes below two million.

```java
    FList<Long> p = Numbers.primes
      .takeWhile(i -> i < 2000000L);
    return Longs.sum(p);
```
Since jFunc comes with an infinite FList of primes we only have to filter the list of all the values above 2000000, then sum the remaining values. The method [takeWhile(P1&lt;A&gt; predicate)](https://drjoliv.github.io/jfunc/drjoliv/jfunc/data/list/FList.html#takeWhile-drjoliv.jfunc.function.P1-) does the filtering and [Longs.sum](https://drjoliv.github.io/jfunc/drjoliv/jfunc/nums/Longs.html#sum-drjoliv.jfunc.data.list.FList-) sums the remaining values.

[Complete Code](https://github.com/drjoliv/ProjectEulerSolutions/blob/master/src/main/java/drjoliv/euler/Euler10.java) |
<a type="button" data-toggle="collapse" data-target="#euler10" aria-expanded="false" aria-controls="euler10">
Answer
</a>
<div class="collapse" id="euler10">
  <div class="well well-sm">142913828922</div>
</div>
