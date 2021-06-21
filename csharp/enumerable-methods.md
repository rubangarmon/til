# Enumerable Methods

All examples and information were taken from [Microsoft documentation](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable?view=net-5.0) and [TutorialsTeacher](https://www.tutorialsteacher.com/linq/linq-standard-query-operators)

| Classification | Standard Query Operators                                                                                                                                                |
| :------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Filtering      | [Where](#where), [OfType](#oftype)                                                                                                                                      |
| Sorting        | [OrderBy](#orderby), [OrderByDescending](#orderbydescending), [ThenBy](#thenby), [ThenByDescending](#thenbydescending), [Reverse](#reverse)                             |
| Grouping       | [GroupBy](#groupby), [ToLookup](#tolookup)                                                                                                                              |
| Join           | [Join](#join), [GroupJoin](#groupjoin)                                                                                                                                  |
| Projection     | [Select](#select), [SelectMany](#selectmany)                                                                                                                            |
| Aggregation    | [Aggregate](#aggregate), [Averange](#averange), [Count](#count-and-longcount), [LongCount](#count-and-longcount), [Max](#max-and-min), [Min](#max-and-min), [Sum](#sum) |
| Quantifiers    | [All](#all), [Any](#any), [Contains](#contains)                                                                                                                         |
| Elements       | [ElementAt](#elementat), [ElementAtOrDefault](#elementatordeafult), First, FirstOrDefault, Last, LastOrDefault, Single, SingleOrDefault                                 |
| Set            | [Distinct](#distinct), [Except](#except), [Intersect](#interset), [Union](#union)                                                                                       |
| Partitioning   | [Skip & SkipLast](#skip--skiplast), [SkipWhile](#skipwhile), Take & TakeLast, [TakeWhile](#takewhile)                                                                   |
| Concatenation  | [Concat](#concat), [Zip](#zip)                                                                                                                                          |
| Equality       | [SequenceEqual](#sequenceequal)                                                                                                                                         |
| Generation     | [DefaultEmpty](#defaultifempty), [Empty](#empty), [Range](#range), [Repeat](#repeat)                                                                                    |
| Conversion     | [AsEnumerable](#asenumerable), AsQueryable, Cast, ToArray, ToDictionary, ToList                                                                                         |

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

## Select

Projects each element of a sequence into a new form.

```csharp
string[] fruits = { "apple", "banana", "mango", "orange",
                      "passionfruit", "grape" };

var query =
    fruits.Select((fruit, index) =>
                      new { index, str = fruit.Substring(0, index) });

foreach (var obj in query)
{
    Console.WriteLine("{0}", obj);
}

/*
 This code produces the following output:

 {index=0, str=}
 {index=1, str=b}
 {index=2, str=ma}
 {index=3, str=ora}
 {index=4, str=pass}
 {index=5, str=grape}
*/

```

## SelectMany

Projects each element of a sequence to an IEnumerable<T> and flattens the resulting sequences into one sequence.

```csharp
class PetOwner
{
    public string Name { get; set; }
    public List<string> Pets { get; set; }
}

public static void SelectManyEx3()
{
    PetOwner[] petOwners =
        { new PetOwner { Name="Higa",
              Pets = new List<string>{ "Scruffy", "Sam" } },
          new PetOwner { Name="Ashkenazi",
              Pets = new List<string>{ "Walker", "Sugar" } },
          new PetOwner { Name="Price",
              Pets = new List<string>{ "Scratches", "Diesel" } },
          new PetOwner { Name="Hines",
              Pets = new List<string>{ "Dusty" } } };

    // Project the pet owner's name and the pet's name.
    var query =
        petOwners
        .SelectMany(petOwner => petOwner.Pets, (petOwner, petName) => new { petOwner, petName })
        .Where(ownerAndPet => ownerAndPet.petName.StartsWith("S"))
        .Select(ownerAndPet =>
                new
                {
                    Owner = ownerAndPet.petOwner.Name,
                    Pet = ownerAndPet.petName
                }
        );

    // Print the results.
    foreach (var obj in query)
    {
        Console.WriteLine(obj);
    }
}

// This code produces the following output:
//
// {Owner=Higa, Pet=Scruffy}
// {Owner=Higa, Pet=Sam}
// {Owner=Ashkenazi, Pet=Sugar}
// {Owner=Price, Pet=Scratches}
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

## _`Averange`_

Average extension method calculates the average of the numeric items in the collection. Average method returns nullable or non-nullable decimal, double or float value.

```csharp
IList<Student> studentList = new List<Student>>() {
        new Student() { StudentID = 1, StudentName = "John", Age = 13} ,
        new Student() { StudentID = 2, StudentName = "Moin",  Age = 21 } ,
        new Student() { StudentID = 3, StudentName = "Bill",  Age = 18 } ,
        new Student() { StudentID = 4, StudentName = "Ram" , Age = 20} ,
        new Student() { StudentID = 5, StudentName = "Ron" , Age = 15 }
    };

var avgAge = studentList.Average(s => s.Age);

Console.WriteLine("Average Age of Student: {0}", avgAge);

//Average Age of Student: 17.4
```

Or using nullable values:

```csharp
long?[] longs = { null, 10007L, 37L, 399846234235L };

double? average = longs.Average();

Console.WriteLine("The average is {0}.", average);

// This code produces the following output:
//
// The average is 133282081426.333.
```

## _`Count`_ and _`LongCount`_

Returns the number of elements in a sequence. _`LongCount`_ returns an _`Int64`_ that represents the number of elements in a sequence. Use it when you expect the result to be greater than _`MaxValue`_.

```csharp
string[] fruits = { "apple", "banana", "mango", "orange", "passionfruit", "grape" };

try
{
    int numberOfFruits = fruits.Count();
    Console.WriteLine(
        "There are {0} fruits in the collection.",
        numberOfFruits);
}
catch (OverflowException)
{
    Console.WriteLine("The count is too large to store as an Int32.");
    Console.WriteLine("Try using the LongCount() method instead.");
}

// This code produces the following output:
//
// There are 6 fruits in the collection.
```

## _`Max`_ and _`Min`_

The Max() method returns the largest numeric element from a collection. Another hand Min() method returns the minimum value in a sequence of values.

Example of Max():

```csharp
double?[] doubles = { null, 1.5E+104, 9E+103, -2E+103 };

double? max = doubles.Max();

Console.WriteLine("The largest number is {0}.", max);

/*
 This code produces the following output:

 The largest number is 1.5E+104.
*/
```

Example of Min()

```csharp
int?[] grades = { 78, 92, null, 99, 37, 81 };

int? min = grades.Min();

Console.WriteLine("The lowest grade is {0}.", min);

/*
 This code produces the following output:

 The lowest grade is 37.
*/
```

## Sum

Compues the sum of a sequence of numeric values.

```csharp
float?[] points = { null, 0, 92.83F, null, 100.0F, 37.46F, 81.1F };

float? sum = points.Sum();

Console.WriteLine("Total points earned: {0}", sum);

/*
 This code produces the following output:

 Total points earned: 311.39
*/
```

## All

Checks if all the elements in a sequence satisfies the specified condition

```csharp
IList<Student> studentList = new List<Student>() {
        new Student() { StudentID = 1, StudentName = "John", Age = 18 } ,
        new Student() { StudentID = 2, StudentName = "Steve",  Age = 15 } ,
        new Student() { StudentID = 3, StudentName = "Bill",  Age = 25 } ,
        new Student() { StudentID = 4, StudentName = "Ram" , Age = 20 } ,
        new Student() { StudentID = 5, StudentName = "Ron" , Age = 19 }
    };

// checks whether all the students are teenagers
bool areAllStudentsTeenAger = studentList.All(s => s.Age > 12 && s.Age < 20);

Console.WriteLine(areAllStudentsTeenAger);

// output
// false

```

## Any

Checks if any of the elements in a sequence satisfies the specified condition

```csharp
bool isAnyStudentTeenAger = studentList.Any(s => s.age > 12 && s.age < 20);
// true
```

## Contains

The Contains operator checks whether a specified element exists in the collection or not and returns a boolean.

```csharp
string fruit = "mango";

bool hasMango = fruits.Contains(fruit);

Console.WriteLine(
    "The array {0} contain '{1}'.",
    hasMango ? "does" : "does not",
    fruit);

// This code produces the following output:
//
// The array does contain 'mango'.
```

Using IEqualityComparer interface

```csharp
    public class Product
    {
        public string Name { get; set; }
        public int Code { get; set; }
    }

    public class ProductComparer : IEqualityComparer<Product>
    {
        public bool Equals(Product x, Product y)
        {
            // Check whether the compared objects reference the same data
            if (Object.ReferenceEquals(x, y)) return true;

            // Check whether any of the compared objects is null
            if (Object.ReferenceEquals(x, null) || Object.ReferenceEquals(y, null))
                return false;

            //Check whether the products' properties are equal
            return x.Code == y.Code && x.Name == y.Name;

        }

        //If Equals() returns true for a pair of objects
        //then GetHasCode() must return the same value for these objects.
        public int GetHashCode(Product obj)
        {
            //Check whether the object is null
            if (Object.ReferenceEquals(obj, null)) return 0;

            //Get hash code for the Name field if it is not null
            int hashProductName = obj.Name == null ? 0 : obj.GetHashCode();

            //Get hash code for the code field
            int hashProductCode = obj.Code.GetHashCode();

            //Calculate the hash code for the product.
            return hashProductName ^ hashProductCode;
        }

    }

    /////////////////////////

    Product[] fruits = {new Product { Code = 9, Name = "apple"},
                new Product { Code = 10, Name = "orange"},
    new Product { Code = 12, Name = "lemon"}};


    var apple = new Product { Name = "apple", Code = 9 };
    var kiwi = new Product { Name = "kiwi", Code = 20 };

    ProductComparer productComparer = new ProductComparer();

    bool hasApple = fruits.Contains(apple);

    /*
    This code produces the following output:

    Apple? True
    Kiwi? False
*/
```

## ElementAt

Returns the element at a specified index in a sequence.

```csharp
string[] names = {"Hartono, Tommy", "Adams, Terry", "Andersen, Henriette Thaulow", "Hedlund, Magnus", "Ito, Shu"};
Random random = new Random(DateTime.Now.Millisecond);

string name = names.ElementAt(random.Next(0, names.length));
Console.WriteLine("The name chosen at random is '{0}.'", name);
/*
    This code produces output similar to the following:
    The name chosen at random is 'Ito, Shu'.
*/
```

## ElementAtOrDeafult

Returns the element at a specified index in a sequence or a default value if the index is out of range.

```csharp
string[] names = { "Hartono, Tommy", "Adams, Terry", "Andersen, Henriette Thaulow",
        "Hedlund, Magnus", "Ito, Shu" };
int index = 20;
string name = names.ElementAtOrDefault(index);

Console.WriteLine(
    "The name chosen at index {0} is {1}",
    index,
    String.IsNullOrEmpty(name) ? "<no name at this index>" : name
);
/*
    This code produces the following output:
    The name chosen at index 20 is '<no name at this index>' .
*/
```

## Distinct

The Distinct extension method returns a new collection of unique elements from the given collection.

```csharp
var strList = new List<string>(){ "One", "Two", "Three", "Two", "Three" };
var intList = new List<int>(){ 1, 2, 3, 2, 4, 4, 3, 5 };

var distinctList1 = strList.Distinct();
foreach(var str in distinctList1)
    Console.WriteLine(str);

var distinctList2 = intList.Distinct();
foreach(var i in distinctList2)
    Console.WriteLine(i);

/*
One
Two
Three
1
2
3
4
5
*/
```

Using IEquatable interface

```csharp
public class Product : IEquatable<Product>
{
    public string Name { get; set; }
    public int Code { get; set; }

    public bool Equals(Product other)
    {

        //Check whether the compared object is null.
        if (Object.ReferenceEquals(other, null)) return false;

        //Check whether the compared object references the same data.
        if (Object.ReferenceEquals(this, other)) return true;

        //Check whether the products' properties are equal.
        return Code.Equals(other.Code) && Name.Equals(other.Name);
    }

    // If Equals() returns true for a pair of objects
    // then GetHashCode() must return the same value for these objects.

    public override int GetHashCode()
    {

        //Get hash code for the Name field if it is not null.
        int hashProductName = Name == null ? 0 : Name.GetHashCode();

        //Get hash code for the Code field.
        int hashProductCode = Code.GetHashCode();

        //Calculate the hash code for the product.
        return hashProductName ^ hashProductCode;
    }
}

/////

Product[] products = { new Product { Name = "apple", Code = 9 },
                       new Product { Name = "orange", Code = 4 },
                       new Product { Name = "apple", Code = 9 },
                       new Product { Name = "lemon", Code = 12 } };

//Exclude duplicates.

IEnumerable<Product> noduplicates =
    products.Distinct();

foreach (var product in noduplicates)
    Console.WriteLine(product.Name + " " + product.Code);

/*
    This code produces the following output:
    apple 9
    orange 4
    lemon 12
*/
```

## Except

The Except() method requires two collections. It returns a new collection with elements from the first collection which do not exist in the second collection (parameter collection).

```csharp
IList<string> strList1 = new List<string>(){"One", "Two", "Three", "Four", "Five" };
IList<string> strList2 = new List<string>(){"Four", "Five", "Six", "Seven", "Eight"};

var result = strList1.Except(strList2);

foreach(string str in result)
        Console.WriteLine(str);

/// output
// One
// Two
// Three
```

using IEquatable interface

```csharp
public class ProductA: IEquatable<ProductA>
{
    public string Name { get; set; }
    public int Code { get; set; }

    public bool Equals(ProductA other)
    {
        if (other is null)
            return false;

        return this.Name == other.Name && this.Code == other.Code;
    }

    public override bool Equals(object obj) => Equals(obj as ProductA);
    public override int GetHashCode() => (Name, Code).GetHashCode();
}

////

ProductA[] fruits1 = { new ProductA { Name = "apple", Code = 9 },
                       new ProductA { Name = "orange", Code = 4 },
                        new ProductA { Name = "lemon", Code = 12 } };

ProductA[] fruits2 = { new ProductA { Name = "apple", Code = 9 } };

//Get all the elements from the first array
//except for the elements from the second array.

IEnumerable<ProductA> except =
    fruits1.Except(fruits2);

foreach (var product in except)
    Console.WriteLine(product.Name + " " + product.Code);

/*
  This code produces the following output:

  orange 4
  lemon 12
*/
```

## Interset

A sequence that contains the elements that form the set intersection of two sequences.

```csharp
IList<string> strList1 = new List<string>() { "One", "Two", "Three", "Four", "Five" };
IList<string> strList2 = new List<string>() { "Four", "Five", "Six", "Seven", "Eight"};

var result = strList1.Intersect(strList2);

foreach(string str in result)
        Console.WriteLine(str);

// output
/*
Four
Five
*/
```

## Union

The Union extension method requires two collections and returns a new collection that includes distinct elements from both the collections. Consider the following example.

```csharp
IList<string> strList1 = new List<string>() { "One", "Two", "three", "Four" };
IList<string> strList2 = new List<string>() { "Two", "THREE", "Four", "Five" };

var result = strList1.Union(strList2);

foreach(string str in result)
        Console.WriteLine(str);

// output
/*
One
Two
three
THREE
Four
Five
*/
```

## Skip & SkipLast

The Skip() method skips the specified number of element starting from first element and returns rest of the elements.

````csharp
var strList = new List<string>(){ "One", "Two", "Three", "Four", "Five" };
var newList = strList.Skip(2);
foreach(var i in newList){
    Console.WriteLine(str);
}
/*
    Three
    Four
    Five
*/

The SkipLast() returns a new enumerable collection that contains the elements from source with the last count elements of the source collection omitted.

```csharp
var numberList = new List<int>() { 23, 34, 44, 45, 56, 566, 2034 };
var newList = numberList.SkipLast(3);
foreach (var item in newList)
{
    Console.WriteLine(item);
}

/*
    23
    34
    44
    45
*/
```


## SkipWhile

Skips elements based on a condition until an element does not satisfy the condition. If the first element itself doesn't satisfy the condition, it then skips 0 elements and returns all the elements in the sequence. FIRST ELEMENT ALWAYS HAS TO BE TRUE.

```csharp
var strList = new List<string>() { "One", "Two", "Three", "Four", "Five", "Six" };

var resultList = strList.SkipWhile(s => s.Length < 4);
foreach(string str in resultList)
    Console.WriteLine(str);
/*
    Three
    Four
    Five
    Six
*/

````

Sorting for the first element to be true.

```csharp
int[] grades = { 59, 82, 70, 56, 92, 98, 85 };

IEnumerable<int> lowerGrades =
    grades
    .OrderByDescending(grade => grade)
    .SkipWhile(grade => grade >= 80);

Console.WriteLine("All grades below 80:");
foreach (int grade in lowerGrades)
{
    Console.WriteLine(grade);
}

/*
 This code produces the following output:

 All grades below 80:
 70
 59
 56
*/
```

## Take

Returns a specified number of contiguous elements from the start of the sequence.

```csharp
var strList = new List<string>() { "One", "Two", "Three", "Four", "Five" };
var newList = strList.Take(2);

foreach(var str in newList)
    Console.WriteLine(str);

/*
    One
    Two
*/
```

# TakeWhile

Returns elements from a sequence as long as a specified condition is true, and then skips the remaining elements.

```csharp
string[] fruits = {"apple", "banana", "mango", "orange", "passionfruit", "grape" };

IEnumerable<string> query = fruits.TakeWhile(fruit => String.Compare("orange", fruit, true) != 0);

foreach(string fruit in query){
    Console.WriteLine(fruit);
};

/*
    apple
    banana
    mango
*/

```

using index

```csharp
string[] fruits = { "apple", "passionfruit", "banana", "mango",
                      "orange", "blueberry", "grape", "strawberry" };

IEnumerable<string> query =
    fruits.TakeWhile((fruit, index) => fruit.Length >= index);

foreach (string fruit in query)
{
    Console.WriteLine(fruit);
}

/*
 This code produces the following output:

 apple
 passionfruit
 banana
 mango
 orange
 blueberry
*/
```

## Concat

The Concat() method appends two sequences of the same type and returns a new sequence (collection).

```csharp
    IList<string> collection1 = new List<string>() { "One", "Two", "Three" };
    IList<string> collection2 = new List<string>() { "Five", "Six"};

    var collection3 = collection1.Concat(collection2);

    foreach (string str in collection3)
        Console.WriteLine(str);
    /*
        One
        Two
        Three
        Five
        Six
    */
```

## Zip

Applies a specified function to the corresponding elements of two sequences, producing a sequence of the results.

```csharp
int[] numbers = { 1, 2, 3, 4 };
string[] words = { "one", "two", "three" };

var numbersAndWords = numbers.Zip(words, (first, second) => first + " " + second);

foreach(var item in numbersAndWords) {
    Console.WriteLine(item);
}

/*
    1 one
    2 two
    3 three
*/
```

## SequenceEqual

Checks whether the number of elements, value of each element and order of elements in two collections are equal or not.
If the collection contains elements of primitive data types then it compares the values and number of elements, whereas collection with complex type elements, checks the references of the objects. So, if the objects have the same reference then they considered as equal otherwise they are considered not equal.

```csharp
Student std = new Student() { StudentID = 1, StudentName = "Bill" };

IList<Student> studentList1 = new List<Student>(){ std };

IList<Student> studentList2 = new List<Student>(){ std };

bool isEqual = studentList1.SequenceEqual(studentList2); // returns true

Student std1 = new Student() { StudentID = 1, StudentName = "Bill" };
Student std2 = new Student() { StudentID = 1, StudentName = "Bill" };

IList<Student> studentList3 = new List<Student>(){ std1};

IList<Student> studentList4 = new List<Student>(){ std2 };

isEqual = studentList3.SequenceEqual(studentList4);// returns false
```

## DefaultIfEmpty

Returns the elements of an IEnumerable<T>, or a default valued singleton collection if the sequence is empty.

```csharp
class Pet{
    public string Name {get; set;}
    public int Age {get; set;}
}

public static void DefaultIfEmptyEx2()
{
    Pet defaultPet = new Pet {Name = "Default Pet", Age = 0 };

    List<Pet> pet1 =
        newList<Pet> {
            new Pet { Name="Barley", Age=8 },
            new Pet { Name="Boots", Age=4 },
            new Pet { Name="Whiskers", Age=1 } };

    foreach (Pet pet in pets1.DefaultIfEmpty(defaultPet))
    {
        Console.WriteLine("Name: {0}", pet.Name);
    }

    List<Pet> pets2 = new List<Pet>();

    foreach (Pet pet in pets2.DefaultIfEmpty(defaultPet))
    {
        Console.WriteLine("\nName: {0}", pet.Name);
    }

/*
    This code produces the following output:

 Name: Barley
 Name: Boots
 Name: Whiskers

 Name: Default Pet

*/

}

```

## Empty

Returns an empty collection

```csharp
var emptyCollection1 = Enumerable.Empty<string>();
var emptyCollection2 = Enumerable.Empty<Student>();

Console.WriteLine("Count: {0}", emptyCollection1.Count());
Console.WriteLine("Type: {0}", emptyCollection1.GetType().Name);

Console.WriteLine("Count: {0}", emptyCollection2.Count());
Console.WriteLine("Type: {0}", emptyCollection2.GetType().Name);

/*
    Count: 0
    Type: String[]
    Count: 0
    Type: Student[]
*/
```

## Range

Generates collection of IEnumerable<T> type with specified number of elements with sequential values, starting from first element.

```csharp
var intCollection = Enumerable.Range(10, 10);
Console.WriteLine("Total Count: {0} ", intCollection.Count());

for(int i = 0; i < intCollection.Count(); i++) {
    Console.WriteLine("Value at index {0} : {1}", i, intCollection.ElementAt(i));
}

/*
    Total Count: 10
Value at index 0 : 10
Value at index 1 : 11
Value at index 2 : 12
Value at index 3 : 13
Value at index 4 : 14
Value at index 5 : 15
Value at index 6 : 16
Value at index 7 : 17
Value at index 8 : 18
Value at index 9 : 19
*/
```

Another example

```csharp
// Generate a sequence of integers from 1 to 10
// and then select their squares.
IEnumerable<int> squares = Enumerable.Range(1, 10).Select(x => x * x);

foreach (int num in squares)
{
    Console.WriteLine(num);
}

/*
 This code produces the following output:

 1
 4
 9
 16
 25
 36
 49
 64
 81
 100
*/
```

## Repeat

Generates a collection of IEnumerable<T> type with specified number of elements and each element contains same specifed value.

```csharp
IEnumerable<string> strings =
    Enumerable.Repeat("I like programming.", 15);

foreach (String str in strings)
{
    Console.WriteLine(str);
}

/*
 This code produces the following output:

 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
 I like programming.
*/
```

## AsEnumerable

Returns the input sequence as IEnumerable<t>. The following code example demonstrates how to use AsEnumerable<TSource>(IEnumerable<TSource>) to hide a type's custom _`Where`_ method whe the standard query operator implementation is desired.

```csharp
// Custom class.
class Clump<T> : List<T>
{
    // Custom implementation of Where().
    public IEnumerable<T> Where(Func<T, bool> predicate)
    {
        Console.WriteLine("In Clump's implementation of Where().");
        return Enumerable.Where(this, predicate);
    }
}

static void AsEnumerableEx1()
{
    // Create a new Clump<T> object.
    Clump<string> fruitClump =
        new Clump<string> { "apple", "passionfruit", "banana",
            "mango", "orange", "blueberry", "grape", "strawberry" };

    // First call to Where():
    // Call Clump's Where() method with a predicate.
    IEnumerable<string> query1 =
        fruitClump.Where(fruit => fruit.Contains("o"));

    Console.WriteLine("query1 has been created.\n");

    // Second call to Where():
    // First call AsEnumerable() to hide Clump's Where() method and thereby
    // force System.Linq.Enumerable's Where() method to be called.
    IEnumerable<string> query2 =
        fruitClump.AsEnumerable().Where(fruit => fruit.Contains("o"));

    // Display the output.
    Console.WriteLine("query2 has been created.");
}

// This code produces the following output:
//
// In Clump's implementation of Where().
// query1 has been created.
//
// query2 has been created.
```
