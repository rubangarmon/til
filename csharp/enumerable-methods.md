# Enumerable Methods

All examples and information were taken from [Microsoft documentation](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable?view=net-5.0) and [TutorialsTeacher](https://www.tutorialsteacher.com/linq/linq-standard-query-operators)

| Classification | Standard Query Operators                                                                                                                    |
| :------------- | :------------------------------------------------------------------------------------------------------------------------------------------ |
| Filtering      | [Where](#where), [OfType](#oftype)                                                                                                          |
| Sorting        | [OrderBy](#orderby), [OrderByDescending](#orderbydescending), [ThenBy](#thenby), [ThenByDescending](#thenbydescending), [Reverse](#reverse) |
| Grouping       | [GroupBy](#groupby), [ToLookup](#tolookup)                                                                                                  |
| Join           | [Join](#join), [GroupJoin](#groupjoin)                                                                                                      |
| Projection     | Select, SelectMany                                                                                                                          |
| Aggregation    | [Aggregate](#aggregate), Averange, Count, LongCount, Max, Min, Sum                                                                          |
| Quantifiers    | All, Any, Contains                                                                                                                          |
| Elements       | ElementAt, ElementAtOrDefault, First, FirstOrDefault, Last, LastOrDefault, Single, SingleOrDefault                                          |
| Set            | Distinct, Except, Intersect, Union                                                                                                          |
| Partitioning   | Skip, SkipWhile, Take, TakeWhile                                                                                                            |
| Concatenation  | Contact, Zip                                                                                                                                |
| Equality       | SequenceEqual                                                                                                                               |
| Generation     | DefaultEmpty, Empty, Range, Repeat                                                                                                          |
| Conversion     | AsEnumerable, AsQueryable, Cast, ToArray, ToDictionary, ToList                                                                              |

## Where

Filters a sequence of values based on a predicate.

```csharp
List<string> fruits =
    new List<string> { "apple", "passionfruit", "banana", "mango",
                    "orange", "blueberry", "grape", "strawberry" };

IEnumerable<string> query = fruits.Where(fruit => fruit.Length < 6);

foreach (string fruit in query)
{
    Console.WriteLine(fruit);
}
/*
 This code produces the following output:

 apple
 mango
 grape
*/
```

## OfType

Filters the elements of an IEnumerable based on a specified type.

```csharp
System.Collections.ArrayList fruits = new System.Collections.ArrayList(4);
fruits.Add("Mango");
fruits.Add("Orange");
fruits.Add("Apple");
fruits.Add(3.0);
fruits.Add("Banana");

// Apply OfType() to the ArrayList.
IEnumerable<string> query1 = fruits.OfType<string>();

Console.WriteLine("Elements of type 'string' are:");
foreach (string fruit in query1)
{
    Console.WriteLine(fruit);
}

// The following query shows that the standard query operators such as
// Where() can be applied to the ArrayList type after calling OfType().
IEnumerable<string> query2 =
    fruits.OfType<string>().Where(fruit => fruit.ToLower().Contains("n"));

Console.WriteLine("\nThe following strings contain 'n':");
foreach (string fruit in query2)
{
    Console.WriteLine(fruit);
}

// This code produces the following output:
//
// Elements of type 'string' are:
// Mango
// Orange
// Apple
// Banana
//
// The following strings contain 'n':
// Mango
// Orange
// Banana
```

## OrderBy

Sorts the elements of a sequence in ascending order.
This method is related with OrderByDescending, ThenBy

<!-- TODO: Links to respective methods -->

```csharp
class Pet
{
    public string Name { get; set; }
    public int Age { get; set; }
}

public static void OrderByEx1()
{
    Pet[] pets = { new Pet { Name="Barley", Age=8 },
                   new Pet { Name="Boots", Age=4 },
                   new Pet { Name="Whiskers", Age=1 } };

    IEnumerable<Pet> query = pets.OrderBy(pet => pet.Age);

    foreach (Pet pet in query)
    {
        Console.WriteLine("{0} - {1}", pet.Name, pet.Age);
    }
}

/*
 This code produces the following output:

 Whiskers - 1
 Boots - 4
 Barley - 8
*/
```

## OrderByDescending

Sorts the elements of a sequence in descending order. The following code example shows how to use _`IComparer<Tkey>`_ parameter with descending order.
This method is related with OrderBy, ThenByDescending

<!-- TODO: Links to respective methods -->

```csharp
/// <summary>
/// This IComparer class sorts by the fractional part of the decimal number.
/// </summary>
public class SpecialComparer : IComparer<decimal>
{
    /// <summary>
    /// Compare two decimal numbers by their fractional parts.
    /// </summary>
    /// <param name="d1">The first decimal to compare.</param>
    /// <param name="d2">The second decimal to compare.</param>
    /// <returns>1 if the first decimal's fractional part
    /// is greater than the second decimal's fractional part,
    /// -1 if the first decimal's fractional
    /// part is less than the second decimal's fractional part,
    /// or the result of calling Decimal.Compare()
    /// if the fractional parts are equal.</returns>
    public int Compare(decimal d1, decimal d2)
    {
        decimal fractional1, fractional2;

        // Get the fractional part of the first number.
        try
        {
            fractional1 = decimal.Remainder(d1, decimal.Floor(d1));
        }
        catch (DivideByZeroException)
        {
            fractional1 = d1;
        }
        // Get the fractional part of the second number.
        try
        {
            fractional2 = decimal.Remainder(d2, decimal.Floor(d2));
        }
        catch (DivideByZeroException)
        {
            fractional2 = d2;
        }

        if (fractional1 == fractional2)
            return Decimal.Compare(d1, d2);
        else if (fractional1 > fractional2)
            return 1;
        else
            return -1;
    }
}

public static void OrderByDescendingEx1()
{
    List<decimal> decimals =
        new List<decimal> { 6.2m, 8.3m, 0.5m, 1.3m, 6.3m, 9.7m };

    IEnumerable<decimal> query =
        decimals.OrderByDescending(num =>
                                       num, new SpecialComparer());

    foreach (decimal num in query)
    {
        Console.WriteLine(num);
    }
}

/*
 This code produces the following output:

 9.7
 0.5
 8.3
 6.3
 1.3
 6.2
*/
```

## ThenBy

Only valid in method syntax. Used for second level sorting in ascending order.

```csharp
string[] fruits = { "grape", "passionfruit", "banana", "mango",
                      "orange", "raspberry", "apple", "blueberry" };

// Sort the strings first by their length and then
//alphabetically by passing the identity selector function.
IEnumerable<string> query =
    fruits.OrderBy(fruit => fruit.Length).ThenBy(fruit => fruit);

foreach (string fruit in query)
{
    Console.WriteLine(fruit);
}

/*
    This code produces the following output:

    apple
    grape
    mango
    banana
    orange
    blueberry
    raspberry
    passionfruit
*/
```

## ThenByDescending

Only valid in method syntax. Used for second level sorting in descending order. The following code example shows how to use _`IComparer<Tkey>`_ parameter.

```csharp
public class CaseInsensitiveComparer : IComparer<string>
{
    public int Compare(string x, string y)
    {
        return string.Compare(x, y, true);
    }
}

public static void ThenByDescendingEx1()
{
    string[] fruits = { "apPLe", "baNanA", "apple", "APple",
        "orange", "BAnana", "ORANGE", "apPLE" };

    // Sort the strings first ascending by their length and
    // then descending using a custom case insensitive comparer.
    IEnumerable<string> query =
        fruits
        .OrderBy(fruit => fruit.Length)
        .ThenByDescending(fruit => fruit, new CaseInsensitiveComparer());

    foreach (string fruit in query)
    {
        Console.WriteLine(fruit);
    }
}

/*
    This code produces the following output:

    apPLe
    apple
    APple
    apPLE
    orange
    ORANGE
    baNanA
    BAnana
*/
```

## Reverse

Inverts the order of the elements in a sequence.

```csharp
char[] apple = {'a', 'p'. 'p', 'l', 'e' };

char[] reversed = apple.Reverse().ToArray();

foreach (char chr in reversed)
{
    Console.Write(chr + " ");
}
Console.WriteLine();
/*
    This code produce the following output:

    e l p p a
*/
```

## GroupBy

The _`GroupBy`_ operator returns groups of elements based on some key value. Each group is represented by _`IGrouping<TKey, TElement>`_ object.

```csharp
IList<Student> studentList = new List<Student>() {
        new Student() { StudentID = 1, StudentName = "John", Age = 18 } ,
        new Student() { StudentID = 2, StudentName = "Steve",  Age = 21 } ,
        new Student() { StudentID = 3, StudentName = "Bill",  Age = 18 } ,
        new Student() { StudentID = 4, StudentName = "Ram" , Age = 20 } ,
        new Student() { StudentID = 5, StudentName = "Abram" , Age = 21 }
    };

var groupedResult = studentList.GroupBy(s => s.Age);

foreach (var ageGroup in groupedResult)
{
    Console.WriteLine("Age Group: {0}", ageGroup.Key);  //Each group has a key

    foreach(Student s in ageGroup)  //Each group has a inner collection
        Console.WriteLine("Student Name: {0}", s.StudentName);
}

/*
    AgeGroup: 18
    StudentName: John
    StudentName: Bill
    AgeGroup: 21
    StudentName: Steve
    StudentName: Abram
    AgeGroup: 20
    StudentName: Ram
*/
```

Creating a new out object

```csharp
class Pet
{
    public string Name { get; set; }
    public double Age { get; set; }
}

public static void GroupByEx4()
{
    // Create a list of pets.
    List<Pet> petsList =
        new List<Pet>{ new Pet { Name="Barley", Age=8.3 },
                       new Pet { Name="Boots", Age=4.9 },
                       new Pet { Name="Whiskers", Age=1.5 },
                       new Pet { Name="Daisy", Age=4.3 } };

    // Group Pet.Age values by the Math.Floor of the age.
    // Then project an anonymous type from each group
    // that consists of the key, the count of the group's
    // elements, and the minimum and maximum age in the group.
    var query = petsList.GroupBy(
        pet => Math.Floor(pet.Age),
        pet => pet.Age,
        (baseAge, ages) => new
        {
            Key = baseAge,
            Count = ages.Count(),
            Min = ages.Min(),
            Max = ages.Max()
        });

    // Iterate over each anonymous type.
    foreach (var result in query)
    {
        Console.WriteLine("\nAge group: " + result.Key);
        Console.WriteLine("Number of pets in this age group: " + result.Count);
        Console.WriteLine("Minimum age: " + result.Min);
        Console.WriteLine("Maximum age: " + result.Max);
    }

    /*  This code produces the following output:

        Age group: 8
        Number of pets in this age group: 1
        Minimum age: 8.3
        Maximum age: 8.3

        Age group: 4
        Number of pets in this age group: 2
        Minimum age: 4.3
        Maximum age: 4.9

        Age group: 1
        Number of pets in this age group: 1
        Minimum age: 1.5
        Maximum age: 1.5
    */
}
```

## ToLookup

Creates a generic _`Lookup<TKey,TElement>`_ from an _`IEnumerable<T>`_. _`ToLookup`_ is the same as _`GroupBy`_; the only difference is _`GroupBy`_ execution is deferred, whereas _`ToLookup`_ execution is immediate. Also, _`ToLookup`_ is only applicable in Method syntax. **_`ToLookup`_ is not supported in the query syntax**.

```csharp
IList<Student> studentList = new List<Student>() {
        new Student() { StudentID = 1, StudentName = "John", Age = 18 } ,
        new Student() { StudentID = 2, StudentName = "Steve",  Age = 21 } ,
        new Student() { StudentID = 3, StudentName = "Bill",  Age = 18 } ,
        new Student() { StudentID = 4, StudentName = "Ram" , Age = 20 } ,
        new Student() { StudentID = 5, StudentName = "Abram" , Age = 21 }
    };

var lookupResult = studentList.ToLookup(s => s.age);

foreach (var group in lookupResult)
{
    Console.WriteLine("Age Group: {0}", group.Key);  //Each group has a key

    foreach(Student s in group)  //Each group has a inner collection
        Console.WriteLine("Student Name: {0}", s.StudentName);
}
/*
    Age Group: 18
    Student Name: John
    Student Name: Bill
    Age Group: 21
    Student Name: Steve
    Student Name: Ron
    Age Group: 20
    Student Name: Ram
*/
```

Using first letter from string name

```csharp
class Package
{
    public string Company { get; set; }
    public double Weight { get; set; }
    public long TrackingNumber { get; set; }
}

public static void ToLookupEx1()
{
    // Create a list of Packages.
    List<Package> packages =
        new List<Package>
            { new Package { Company = "Coho Vineyard",
                  Weight = 25.2, TrackingNumber = 89453312L },
              new Package { Company = "Lucerne Publishing",
                  Weight = 18.7, TrackingNumber = 89112755L },
              new Package { Company = "Wingtip Toys",
                  Weight = 6.0, TrackingNumber = 299456122L },
              new Package { Company = "Contoso Pharmaceuticals",
                  Weight = 9.3, TrackingNumber = 670053128L },
              new Package { Company = "Wide World Importers",
                  Weight = 33.8, TrackingNumber = 4665518773L } };

    // Create a Lookup to organize the packages.
    // Use the first character of Company as the key value.
    // Select Company appended to TrackingNumber
    // as the element values of the Lookup.
    ILookup<char, string> lookup =
        packages
        .ToLookup(p => Convert.ToChar(p.Company.Substring(0, 1)),
                  p => p.Company + " " + p.TrackingNumber);

    // Iterate through each IGrouping in the Lookup.
    foreach (IGrouping<char, string> packageGroup in lookup)
    {
        // Print the key value of the IGrouping.
        Console.WriteLine(packageGroup.Key);
        // Iterate through each value in the
        // IGrouping and print its value.
        foreach (string str in packageGroup)
            Console.WriteLine("    {0}", str);
    }
}

/*
 This code produces the following output:

 C
     Coho Vineyard 89453312
     Contoso Pharmaceuticals 670053128
 L
     Lucerne Publishing 89112755
 W
     Wingtip Toys 299456122
     Wide World Importers 4665518773
*/
```

### _`GroupBy`_ or _`ToLookup`_

Taken from [stackoverflow's answer](https://stackoverflow.com/questions/10215428/why-are-tolookup-and-groupby-different)

> Logically they are the same thing but the performance implications of each are completely different. Calling _`ToLookup`_ means _I want a cache of the entire thing right now organized by group_. Calling _`GroupBy`_ means _"I am building an object to represent the question 'what would these things look like if I organized them by group?'"_

## Join

The Join operator joins two sequences (collections) based on a key and returns a resulted sequence. It is the same as _`inner join`_ of SQL.

Simple join

```csharp
IList<string> strList1 = new List<string>() {
    "One",
    "Two",
    "Three",
    "Four"
};

IList<string> strList2 = new List<string>() {
    "One",
    "Two",
    "Five",
    "Six"
};

var innerJoin = strList1.Join(strList2,
                      str1 => str1,
                      str2 => str2,
                      (str1, str2) => str1);

/*
    One
    Two
*/

```

Using property which matches with Student and Standard classes

```csharp
public class Student{
    public int StudentID { get; set; }
    public string StudentName { get; set; }
    public int StandardID { get; set; }
}

public class Standard{
    public int StandardID { get; set; }
    public string StandardName { get; set; }
}

///

IList<Student> studentList = new List<Student>() {
    new Student() { StudentID = 1, StudentName = "John", StandardID =1 },
    new Student() { StudentID = 2, StudentName = "Moin", StandardID =1 },
    new Student() { StudentID = 3, StudentName = "Bill", StandardID =2 },
    new Student() { StudentID = 4, StudentName = "Ram" , StandardID =2 },
    new Student() { StudentID = 5, StudentName = "Ron"  }
};

IList<Standard> standardList = new List<Standard>() {
    new Standard(){ StandardID = 1, StandardName="Standard 1"},
    new Standard(){ StandardID = 2, StandardName="Standard 2"},
    new Standard(){ StandardID = 3, StandardName="Standard 3"}
};

var innerJoin = studentList.Join(// outer sequence
                      standardList,  // inner sequence
                      student => student.StandardID,    // outerKeySelector
                      standard => standard.StandardID,  // innerKeySelector
                      (student, standard) => new  // result selector
                                    {
                                        StudentName = student.StudentName,
                                        StandardName = standard.StandardName
                                    });

foreach (var obj in innerJoin)
{

    Console.WriteLine("{0} - {1}", obj.StudentName, obj.StandardName);
}
/*
John - Standard 1
Steve - Standard 1
Bill - Standard 2
Ram - Standard 2
*/
```

Antoher join using object as key

```csharp
class Person
{
    public string Name { get; set; }
}

class Pet
{
    public string Name { get; set; }
    public Person Owner { get; set; }
}

public static void JoinEx1()
{
    Person magnus = new Person { Name = "Hedlund, Magnus" };
    Person terry = new Person { Name = "Adams, Terry" };
    Person charlotte = new Person { Name = "Weiss, Charlotte" };

    Pet barley = new Pet { Name = "Barley", Owner = terry };
    Pet boots = new Pet { Name = "Boots", Owner = terry };
    Pet whiskers = new Pet { Name = "Whiskers", Owner = charlotte };
    Pet daisy = new Pet { Name = "Daisy", Owner = magnus };

    List<Person> people = new List<Person> { magnus, terry, charlotte };
    List<Pet> pets = new List<Pet> { barley, boots, whiskers, daisy };

    // Create a list of Person-Pet pairs where
    // each element is an anonymous type that contains a
    // Pet's name and the name of the Person that owns the Pet.
    var query =
        people.Join(pets,
                    person => person,
                    pet => pet.Owner,
                    (person, pet) =>
                        new { OwnerName = person.Name, Pet = pet.Name });

    foreach (var obj in query)
    {
        Console.WriteLine(
            "{0} - {1}",
            obj.OwnerName,
            obj.Pet);
    }
}

/*
 This code produces the following output:

 Hedlund, Magnus - Daisy
 Adams, Terry - Barley
 Adams, Terry - Boots
 Weiss, Charlotte - Whiskers
*/
```

```csharp

```

## GroupJoin

The GroupJoin operator joins two sequences based on keys and returns groups of sequences. **It is like Left Outer Join of SQL**. This performs the same task as Join operator except that GroupJoin returns a result in group based on specified group key. The GroupJoin operator joins two sequences based on key and groups the result by matching key and then returns the collection of grouped result and key.

```csharp
public class Student{
    public int StudentID { get; set; }
    public string StudentName { get; set; }
    public int StandardID { get; set; }
}

public class Standard{
    public int StandardID { get; set; }
    public string StandardName { get; set; }
}

////

IList<Student> studentList = new List<Student>() {
    new Student() { StudentID = 1, StudentName = "John", StandardID =1 },
    new Student() { StudentID = 2, StudentName = "Moin", StandardID =1 },
    new Student() { StudentID = 3, StudentName = "Bill", StandardID =2 },
    new Student() { StudentID = 4, StudentName = "Ram",  StandardID =2 },
    new Student() { StudentID = 5, StudentName = "Ron" }
};

IList<Standard> standardList = new List<Standard>() {
    new Standard(){ StandardID = 1, StandardName="Standard 1"},
    new Standard(){ StandardID = 2, StandardName="Standard 2"},
    new Standard(){ StandardID = 3, StandardName="Standard 3"}
};

var groupJoin = standardList.GroupJoin(studentList,  //inner sequence
                                std => std.StandardID, //outerKeySelector
                                s => s.StandardID,     //innerKeySelector
                                (std, studentsGroup) => new // resultSelector
                                {
                                    Students = studentsGroup,
                                    StandarFulldName = std.StandardName
                                });

foreach (var item in groupJoin)
{
    Console.WriteLine(item.StandarFulldName );

    foreach(var stud in item.Students)
        Console.WriteLine(stud.StudentName);
}

/*
    Standard 1:
    John,
    Moin,
    Standard 2:
    Bill,
    Ram,
    Standard 3:
*/

```

Another example:
```csharp
class Person
{
    public string Name { get; set; }
}

class Pet
{
    public string Name { get; set; }
    public Person Owner { get; set; }
}

public static void GroupJoinEx1()
{
    Person magnus = new Person { Name = "Hedlund, Magnus" };
    Person terry = new Person { Name = "Adams, Terry" };
    Person charlotte = new Person { Name = "Weiss, Charlotte" };

    Pet barley = new Pet { Name = "Barley", Owner = terry };
    Pet boots = new Pet { Name = "Boots", Owner = terry };
    Pet whiskers = new Pet { Name = "Whiskers", Owner = charlotte };
    Pet daisy = new Pet { Name = "Daisy", Owner = magnus };

    List<Person> people = new List<Person> { magnus, terry, charlotte };
    List<Pet> pets = new List<Pet> { barley, boots, whiskers, daisy };

    // Create a list where each element is an anonymous
    // type that contains a person's name and
    // a collection of names of the pets they own.
    var query =
        people.GroupJoin(pets,
                         person => person,
                         pet => pet.Owner,
                         (person, petCollection) =>
                             new
                             {
                                 OwnerName = person.Name,
                                 Pets = petCollection.Select(pet => pet.Name)
                             });

    foreach (var obj in query)
    {
        // Output the owner's name.
        Console.WriteLine("{0}:", obj.OwnerName);
        // Output each of the owner's pet's names.
        foreach (string pet in obj.Pets)
        {
            Console.WriteLine("  {0}", pet);
        }
    }
}

/*
 This code produces the following output:

 Hedlund, Magnus:
   Daisy
 Adams, Terry:
   Barley
   Boots
 Weiss, Charlotte:
   Whiskers
*/
```

For this kind of query is better to use Query Syntax

```csharp
from ... in outerSequence

join ... in innerSequence

on outerKey equals innerKey

into groupedCollection

select ...
```

```csharp
IList<Student> studentList = new List<Student>() {
                    new Student() { StudentID = 1, StudentName = "John", Age = 13, StandardID =1 },
                    new Student() { StudentID = 2, StudentName = "Moin",  Age = 21, StandardID =1 },
                    new Student() { StudentID = 3, StudentName = "Bill",  Age = 18, StandardID =2 },
                    new Student() { StudentID = 4, StudentName = "Ram" , Age = 20, StandardID =2 },
                    new Student() { StudentID = 5, StudentName = "Ron" , Age = 15 }
};

IList<Standard> standardList = new List<Standard>() {
                    new Standard(){ StandardID = 1, StandardName="Standard 1"},
                    new Standard(){ StandardID = 2, StandardName="Standard 2"},
                    new Standard(){ StandardID = 3, StandardName="Standard 3"}
};

var groupJoin = from std in standardList
                    join s in studentList
                    on std.StandardID equals s.StandardID
                    into studentGroup
                    select new {
                              Students = studentGroup ,
                              StandardName = std.StandardName
                          };

foreach (var item in groupJoin)
{
                    Console.WriteLine(item.StandarFulldName );

                    foreach(var stud in item.Students)
                    Console.WriteLine(stud.StudentName);
}

/*
    Standard 1:
    John,
    Moin,
    Standard 2:
    Bill,
    Ram,
    Standard 3:
*/
```
## _`Aggregate`_

Applies an accumulator function over a sequence. The specified seed value is used as the initial accumulator value, and the specified function is used to select the result value.

```csharp
string[] fruits = { "apple", "mango", "orange", "passionfruit", "grape" };

// Determine whether any string in the array is longer than "banana"<-- seed value
string longestName =
    fruits.Aggregate("banana",
                    (longest, next) =>
                        next.Length > longest.Length ? next : longest,
    // Return the final result as an upper case string.
                    fruit => fruit.ToUpper());

Console.WriteLine(
    "The fruit with the longest name is {0}.",
    longestName);

// The fruit with the longest name is PASSIONFRUIT.
```

To make a counter

```csharp
int[] ints = { 4, 8, 8, 3, 9, 0, 7, 8, 2 };

// Count the even numbers in the array, using a seed value of 0.
int numEven = ints.Aggregate(0, (total, next) =>
                                    next % 2 == 0 ? total + 1 : total);

Console.WriteLine("The number of even integers is: {0}", numEven);

// This code produces the following output:
//
// The number of even integers is: 6
```

To reverse a sentence

```csharp
string sentence = "the quick brown fox jumps over the lazy dog";

// Split the string into individual words.
string[] words = sentence.Split(' ');

// Prepend each word to the beginning of the
// new sentence to reverse the word order.
string reversed = words.Aggregate((workingSentence, next) =>
                                      next + " " + workingSentence);

Console.WriteLine(reversed);

// This code produces the following output:
//
// dog lazy the over jumps fox brown quick the
```
