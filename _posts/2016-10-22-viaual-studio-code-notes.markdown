---
layout: post
title:  "Visual Studio Code Notes"
date:   2016-10-22 15:48:00 +0100
categories: jekyll update
---

<table>
  <tbody>
    <tr><td>cmd+shift+p</td><td> - Command Palette</td></tr>
    <tr><td>cmd+p</td><td> - Navigate to file</td></tr>
    <tr><td>cmd+p ?</td><td> - List actions available</td></tr>
    <tr><td>cmd+shift+o</td><td> - Symbol palette</td></tr>
    <tr><td>cmd+p #</td><td> - Find symbol by name</td></tr>
    <tr><td>cmd+shift+m</td><td> - Show Errors</td></tr>
    <tr><td>cmd+shift+n</td><td> - New instance of Visual Studio Code</td></tr>
    <tr><td>cmd+n</td><td> - New file</td></tr>
    <tr><td>cmd+\</td><td> - Split editor</td></tr>
    <tr><td>cmd+B</td><td> - Toggle sidebar</td></tr>
    <tr><td>cmd+space</td><td> - get intellisense</td></tr>
    <tr><td>alt+up arrow</td><td> - move line up</td></tr>
    <tr><td>alt+down arrow</td><td> - move line down</td></tr>
    <tr><td>F8</td><td> - Go to next error or warning</td></tr>
    <tr><td>F12</td><td> - Go to definition</td></tr>
    <tr><td>alt+F12</td><td> - Peek definition</td></tr>
    <tr><td>shift+F12</td><td> - Find all references</td></tr>
    <tr><td>cmd+F12</td><td> - Change all occurences</td></tr>
    <tr><td>click+alt</td><td> - Multiple cursors</td></tr>
    <tr><td>F2</td><td> - Rename symbol in all files</td></tr>
    <tr><td>F5</td><td> - Start debugging</td></tr>
    <tr><td>F10</td><td> - Step over</td></tr>
    <tr><td>F11</td><td> - Step into</td></tr>
    <tr><td>shift+F11</td><td> - Step out</td></tr>
    <tr><td>shift+F5</td><td> - stop</td></tr>
  </tbody>
</table>

## intellisense

* List members
* Parameter info
* Quick info
* Complete Word

### List members

List of valid members after you type a .  if you continue to type after the list has bee displayd the list is filtered.


### Parameter info

Information about the parameters a mehtod expects.


### Quick info

Explains what a method does.


### Complete Word

Can complete a variable/member name after enough charaters have been typed to make it disambiguous. 

## References

https://johnpapa.net/getting-started-with-visual-studio-code/
https://johnpapa.net/intellisense-witha-visual-studio-code/
https://johnpapa.net/refactoring-with-visual-studio-code/
https://msdn.microsoft.com/en-us/library/hcw1s69b.aspx

## What is a set?

A set is an unordered collection of _zero or more objects ( object may also be refered to as member or element )_ without duplication. 

e.g.

{ 1,2,3 } and { 1,1,1,2,2,2,3 } represent the same set because the duplicates are ignored. 


## Set membership

 o ∈ A

States that o is a member of the set labled A.


### Set definition

A set can be defined: 

* explicitly - { 1,2,3 } 
* explicitly with sequence continuations - { ...,1,2,3,... }
* via set builder notation - { x ∈ R \| x > 0 } ( positive real numbers )


## Equality

Two sets are equal if an only if they have the same members.

A = B if and only if ∀ x [ x ∈ A <-> x ∈ B ]


## Subsets

If all the members of a set are also members of another set then that set is a **subset** of the other set.  This implies that all sets are **subsets** of themselves to avoid this potentially infinite recursion there is the concept of a **proper subset** which states that all member of the first set are members of another set but the two set are not equal.

* A ⊆ B  - States that A is a _subset_ of B. 
* A ⊂ B  - State that A is a _proper subset_ of B.



## Union

The union of two sets is the set that contains all _distinct_ objects from the two sets.  The union of A and B is written **"A ∪ B."**.  If there are any objects that are in both A and B they will only appear once in the union of the two sets.

e.g.

* A = { 1,2,3,4,5 }
* B = { 4,5,6,7,8 }
* A ∪ B = { 1,2,3,4,5,6,7,8 }


###  SQL

Union can be performed in sql via the following statement:

```
-- Example using MySql

create table `ints` (
	`value` int(11),
    primary key( `value` )
) engine=InnoDB default charset=big5;


insert into ints
values ( 1 ), ( 2 ), (3 ), (4), (5), (6),(7),(8) ;


select * from ints where value < 6
 union 
select * from ints where value > 3 

```

**Note** - with SQL if you wish to include duplicates you can use Union All. This is likely to be more efficient ( although less explicit if you actuall want _union_ ) as it does not have to search through the union and filter out duplicate rows.  You have to have the same number of columns and each column has to be of the same type but they do not have to be named the same.  


### Linq 

Union can be performed using linq, a c# example is:

```
using System;
using System.Collections.Generic;
using System.Linq;

namespace ConsoleApplication {

    public class Program {

        public static void Main( string[] args ) {
            
            Console.WriteLine( "Linq Sets!" );           
            
            var A = new [] { 1,2,3,4,5 };
            var B = new [] { 4,5,6,7,8 };
                  
            var union = A.Union( B );
            
            Console.WriteLine( $"Union : {to_space_separated_string( union )}" ); // 1 2 3 4 5 6 7 8
        }

        private static string to_space_separated_string
                                ( IEnumerable<int> list ) {
                               
          return 
            list
              .Aggregate(
                  ""
                 ,( acc, c ) => acc + c + " " 
               );
        }

    }
}

```


## Intersection

The intersection of two sets is the set that contains all objects that are in both A and B. The intersection of A and B is written **"A ∩ B."**

e.g.

* A = { 1,2,3,4,5 }
* B = { 4,5,6,7,8 }
* A ∩ B = { 4,5 }

### SQL

Intersection can be performed in sql via the following statement:

```
-- Example using MySql

create table `ints` (
	`value` int(11),
    primary key( `value` )
) engine=InnoDB default charset=big5;


insert into ints
values ( 1 ), ( 2 ), (3 ), (4), (5), (6),(7),(8) ;

select *
 from 
  (select * from ints where value < 6) A
 join 
  (select * from ints where value > 3) B
  on A.value = B.value

```

# Linq

Intersection can be performed in Linq, a c# example follows: 

```
using System;
using System.Collections.Generic;
using System.Linq;

namespace ConsoleApplication {

    public class Program {

        public static void Main( string[] args ) {
            
            Console.WriteLine( "Linq Sets!" );           

            var A = new [] { 1,2,3,4,5 };
            var B = new [] { 4,5,6,7,8 };
                   
            var intersect = A.Intersect( B );
            
            Console.WriteLine( $"Intersection: {to_space_separated_string( intersect )}" );  // 4 5
        }

        private static string to_space_separated_string
                                ( IEnumerable<int> list ) {
                               
          return 
            list
              .Aggregate(
                  ""
                 ,( acc, c ) => acc + c + " " 
               );
        }  
    }
}

```

Intersect uese the default equality comparer to compare values.


## Complement ( Set difference )

The complement of set A with respect to set B contains objects that are in B but not in A.  The complement of A with respect to B is written **B \ A**.  

**Note** - this is a bit back to front when written in english because you are asking for a subset of the sentence object ( set B ) rather than the sentence subject ( set A ).

e.g.

* A = { 1,2,3,4,5 }
* B = { 4,5,6,7,8 }
* B \ A = { 6,7,8 }

### SQL

```
-- Example using MySql 


create table `ints` (
	`value` int(11),
    primary key( `value` )
) engine=InnoDB default charset=big5;


insert into ints
values (1), (2), (3), (4), (5), (6),(7),(8) ;



/*
  B \ A :: 
  
  Complemet of A with respect to B which is the 
  set containing the values that are in B but not in A

*/
select * 
  
  from ( 
     select value from ints where value > 3
  ) B
  
  where  
    value not in (
	   select value from ints where value < 6 
	);
    
  -- 6,7,8

/*    
  A \ B :: 
  
  Complement of B with respect A which is the 
  set containing the values that are in A but not in B
  
*/
select *

  from (
    select value from ints where value < 6
  ) A

  where
    value not in (
       select value from ints where value > 3
    )
    
  --- 1,2,3  
```

### Linq

```
using System;
using System.Collections.Generic;
using System.Linq;

namespace ConsoleApplication {

    public class Program {

        public static void Main( string[] args ) {

            Console.WriteLine( "Linq Sets!" );           
            
            var A = new [] { 1,2,3,4,5 };
            var B = new [] { 4,5,6,7,8 };
                   
            var complement_ba = B.Except( A );
            var complement_ab = A.Except( B );
                                
            Console.WriteLine( $@"Complement B \ A: {to_space_separated_string( complement_ba )}" ); // 6,7,8
            Console.WriteLine( $@"Complement A \ B: {to_space_separated_string( complement_ab )}" ); // 1,2,3
        }

        private static string to_space_separated_string
                                ( IEnumerable<int> list ) {
                               
          return 
            list
              .Aggregate(
                  ""
                 ,( acc, c ) => acc + c + " " 
               );
        }   
    }
}
```


## Symetric difference ( disjunctive union )

The symmetric difference of two sets is a set that contains objects that are in either set but not both set. The symmetric difference of A and B is written **A Δ B**.

The symetric difference is worked out as the union of the complement of the two set **A Δ B = (A \ B)  U ( B \ A )**. 

e.g.

* A = { 1,2,3,4,5 }
* B = { 4,5,6,7,8 }
* B Δ A = { 1,2,3,6,7,8 }

**Note** -  The symetirc difference of three or more sets is the symmetric difference of the last set and the set that is the symetric difference of the first two.  This is important to note because if 4 is in all three sets it will be included in the final set because the symmetric difference of the firts two sets will exclude it so when performing the semetric difference between the symmetric difference of the first two set and the last set 4 will be included as it is only in the last set.


### SQL

```
-- Example using MySql 

create table `ints` (
	`value` int(11),
    primary key( `value` )
) engine=InnoDB default charset=big5;


insert into ints
values (1), (2), (3), (4), (5), (6), (7), (8) ;


select *
from(
 
select *   
  from ( 
     select value from ints where value > 3
  ) B
  where  
    value not in (
	   select value from ints where value < 6 
	)
 ) BA
 
union ( 

select *
  from (
    select value from ints where value < 6
  ) A
  where
    value not in (
       select value from ints where value > 3
    )

)
order by value
```

### Linq

```
using System;
using System.Collections.Generic;
using System.Linq;

namespace ConsoleApplication {
    
    public class Program {
        public static void Main( string[] args ) {

            Console.WriteLine( "Linq Sets!" );           
            
            var A = new [] { 1,2,3,4,5 };
            var B = new [] { 4,5,6,7,8 };
                   
            var complement_ba = B.Except( A );
            var complement_ab = A.Except( B );
            
            var symmetric_difference = complement_ab.Union( complement_ba );
            
                                
            Console.WriteLine( $@"symmetric difference: {to_space_separated_string( symmetric_difference )}" ); // 6,7,8

        }
        
        private static string to_space_separated_string
                                ( IEnumerable<int> list ) {
                               
          return 
            list
              .Aggregate(
                  ""
                 ,( acc, c ) => acc + c + " " 
               );
        }        

    }
}

```

## Cartesian product

Returns a set of ordered pairs for two sets. That is for set A and B the cartesian product **A x B** is the set of all ordered pairs ( a, b ) where a ∈ A and b ∈ B.

e.g.

* A = { 1,2,3 }
* B = { 4,5,6 }
* A x B = {(1,4), (1,5), (1,6), (2,4), (2,5), (2,6), (3,4), (3,5), (3,6)}

### SQL

```
-- Example using MySql 


create table `ints` (
	`value` int(11),
    primary key( `value` )
) engine=InnoDB default charset=big5;


insert into ints
values (1), (2), (3), (4), (5), (6), (7), (8) ;


select *
 from 
  (select * from ints where value < 6) A,
  (select * from ints where value > 3) B
order by A.value, B.value
```

### Linq

```
using System;
using System.Collections.Generic;
using System.Linq;

namespace ConsoleApplication {
    
    public class Program {

        public static void Main( string[] args ) {

            Console.WriteLine( "Linq Sets!" );           

            var cartesian_product = new CartesianProduct();

            Console.WriteLine( cartesian_product.generate() );            

        }
    }

    public class CartesianProduct {

        private class Product {  public int a; public int b; };

        public string generate() {
            var A = new [] { 1,2,3,4,5 };
            var B = new [] { 4,5,6,7,8 };

            var cartesian_product 
                 = generate_product(
                      A
                     ,B
                 ); 

            return as_string( cartesian_product );

        }

        private IEnumerable<Product> generate_product 
                                       ( IEnumerable<int> A 
                                       , IEnumerable<int> B ) {

          if ( A == null ) throw new ArgumentNullException( "A" );
          if ( B == null ) throw new ArgumentNullException( "B" );


          return
            A.SelectMany( a => 
              B.Select( b =>
                new Product { a=a, b=b }
              )
            );
        }


        private string as_string
                        (  IEnumerable<Product> cartesian_product ) {

          return 
            cartesian_product
              .Aggregate(
                 ""
                ,( acc, p ) =>
                    acc + $"({p.a}, {p.b}) "
              );
        }
    }

}

```

## Cardinality

Is the measure of the number of objects in the set.  **Note** if a set is defined with duplicates the duplicates objects are only counted once. A frequent notation for the cardinality of set A is \|A\|.

e.g.

\|{ 1,2,2,3 }\| cardinality is 3.

Because:

{1,1,1,2,2} can be described as {1} U {1} U {1} U {2} U {2} 
 U {1} U {1} U {1} U {2} U {2}  = {1, 2}

There are two approaches to cardinality:

* By comparing set directly using bijection.
* By use of cardinal numbers.

### By comparing sets directly using bijection 

Two sets are said to be of the same cardinality if there exists a 1-1 correspondence between the two. In other words two sets A and B have the same cardinality if there exists a _bijection_ from A to B.

A ≈ B or A ~ B 

E = { 0, 2, 4, 6, ... } ( non negative even numbers )
N = { 0, 1, 2, 3, ... } ( natural numbers ) 

E has the same cardinality as N since the _function f(n) = 2n_ is a bijection from N to E.

A _bijection_ is a function that is _injective_ and _surjective_.

#### injective functions

f(x) -> y

f is an injective function if it preserves distinctness, it never maps distinct elements of its domain (x) to the elements of its co-domain (y).


#### surjective functions

f(x) -> y

f is a surjective function if every element of the co-domain (y) has a corresponding element in the domain (x).  **Note** they do not have to be distinct.

### By use of cardinal numbers.

Two finite sets have the same cardinality only if they have the same number of elements. Their common number of elements serves to denote their cardinality. 
So the term finite cardinal number is a synonym for natural number. The cardinality of the set {1, 2, 3, 4, 5} is 5. 

**Note** presumably the the bijection is done by representing each element in the sets with a natural number.


## Empty set

This is the set that contains no objects.


## Power set

The power set of a set is the set of all possible subsets of S including the empty set and the set itself.

It is denoted in several ways:

* P(S)
* ℘(S)
* P(S)
* ℙ(S)
* 2<sup>S</sup>

### Construction of the power set 

The power set of a set can be constructed using binary vectors:

S = { a,b,c}

<table>
  <thead>
    <tr><th>a</th><th>b</th><th>c</th><th>result</th></tr>
  </thead>

  <tbody>
    <tr><td>0</td><td>0</td><td>0</td><td>{}</td></tr>
    <tr><td>0</td><td>0</td><td>1</td><td>{c}</td></tr>
    <tr><td>0</td><td>1</td><td>0</td><td>{b}</td></tr>
    <tr><td>0</td><td>1</td><td>1</td><td>{b,c}</td></tr>
    <tr><td>1</td><td>0</td><td>0</td><td>{a}</td></tr>
    <tr><td>1</td><td>0</td><td>1</td><td>{a,c}</td></tr>
    <tr><td>1</td><td>1</td><td>0</td><td>{a,b}</td></tr>
    <tr><td>1</td><td>1</td><td>1</td><td>{a,b,c}</td></tr>
  </tbody>
</table>

The power set = { {}, {c}, {b}, {b,c}, {a}, {a,c}, {a,b}, {a,b,c} }. 