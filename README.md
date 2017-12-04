# example-aspnet-collections
#### Example of various data collections ####

C# Collections
https://teamtreehouse.com/library/jagged-arrays

**Arrays**

- Creating an example spreadsheet object.
- All arrays must specify the size.
- Arrays are fast for storing and updating because the data are stored synchronously in memory.  But bad for adding and removing items since it requires allocating memory slots.

```csharp
class Cell {
  public string Contents { get; set; }
}

/* 
causes error Cell[][] sheet = new Cell[100][10]; 
because compiler thinks we're instantiating an array and then access item in index 10 and assigning to the sheet variable
*/

Cell[][] sheet = new Cell[100][];

// instead to do an array of 100 containing an array of 10 you have to run an array, this is called creating a jagged array since you can have the inner array be of various size

for (int i = 0; i < sheet.Length; i++) {
	sheet[i] = new Cell[10];
    for(int ci = 0; ci < sheet[i].Length; ci++) {
    	sheet[i][ci] = new Cell();
    }
}

// foreach output example
foreach(Cell[] row in sheet) {
	foreach(Cell cell in row) {
    	System.Console.Write(cell);
    }
    System.Console.WriteLine();
}

// multidimensional arrays
Cell[,] sheet = new Cell[100,10];
Console.Write(sheet.Length) //1000

// multidimensional array of cells
for(int i = 0; i < sheet.GetLength(0); i++) { //100
	for ci = 0; ci < sheet.GetLength(1); ci++) { //10
    	sheet[i,ci] = new Cell(); //assign each index with cell obj
    }
}

// you can't use nested foreach loop to print out the values with multidimensional unlike a jagged array since the multidimensional array is just one large array that is subdivided into equal size parts.  so there's no inner looping to do.

// 3-dimensional jagged array - with this jagged array you still need to loop through and initalize them.  
Cell [][][] sheet = new Cell[100][][];

// 3-dimensional array.  You can do up to 32 dimensions.
int [,,] threeDimMatrix = new int[5,5,5];

// Jagged + multidimensional array = array of 5 3-dimensional array of integer arrays
int[][,,][] yikes = new int[5][,,][];
```

- Array have a fix length and stored synchronously, so because of this it's bad for changing the size.  Look into `System.Array` for some built in methods.

```csharp
int[] ages = { 24, 31, 56 };
int[] agesCopy = new int[4]; //create new array with the size we want

// copy ages to agesCopy starting at index 1 which means index 0 is open to be inserted
ages.CopyTo(agesCopy, 1); 

// insert at 0
agesCopy[0] = 16

// same thing if you want to insert at the end.  a pain and very slow process.
```

**Lists**

- Lists have an Add method that allows easy insert to the end of the list.
- A list is just a wrapper around the array object so its bound to the same shortfalls of array.  When an array that the list is using is out of space, it creates a new array with twice the size of the old array behind the scene.  But the wrapper takes care of this as well as any associated reference to that array.  Best to set the capacity of the list if possible.

```csharp
List<string> students = new List<string>(); // best to set a number for the capacity
//students.Capacity; // 0
students.Add("Angela");
students.Add("Bill");
//students.Capabity; // 4
students.Add("Charles");
students.Add("Dan");
//students.Capabity; // 4
students.Add("Ed");
students.Add("Fred");
//students.Capabity; // 8
```
- `students.Remove("Ed")` will return true and only remove the first instance of Ed.  The search is slow and the rearrangement can be slow if its near the beginning.  This can be sped up by sorting a list.
- `students.Sort()` in place sort and sort by alphabetically / ascending.  To customize the sort, look into `IComparable interface`

```csharp
class Student : System.IComparable<Student> {
	public string Name { get; set; }
    public int GradeLevel { get; set; }
    
    public int CompareTo(Student that) {
    	int result = this.Name.CompareTo(that.Name);
        
        if (result == 0) {
        	result = this.GradeLevel.CompareTo(that.GradeLevel);
        }
        return result;
    }
}

// in main()
List<Student> students = new List<Student> {
	new Student() { Name = "Sally", GradeLevel = 3 },
    new Student() { Name = "Timmy", GradeLevel = 3 },
    new Student() { Name = "Billy", GradeLevel = 2 },
}

students.Sort();

foreach(Student student in students) {
	Console.WriteLine($"{student.Name} is in grade {student.GradeLevel}");
}
```

