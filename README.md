# Morten la Cour's *C# tutorial*

## table of content
- [Collections](#collections)






## Collections

### foreach
Many people mistakenly believes that the foreach *code sugar* syntax requires the object iterated through to implement the *IEnumable interface* 
The worst part is that **Microsoft** themself states in the official documentation that this is indeed the case:
[https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/foreach-in](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/foreach-in)
> The foreach statement repeats a group of embedded statements for each element in an array or an object collection that implements the System.Collections.IEnumerable or System.Collections.Generic.IEnumerable<T> interface. 

Now don't get me wrong, if you want to create a new **class** type that a *foreach* can loop through, you should definetly implement the *IEnumable* interface, since doing so will ensure that everything is set up correctly for using the *foreach* syntax. However, behind the scene, the *foreach* will **NEVER** look for this *interface*, but only for a single *method* named _**GetEnumerator**_

Let's see this in action. I'm going to make a small stupid *C# class* and try to use *foreach* with it:

```csharp
    class ICanDoForEach
    {

    }
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                ICanDoForEach iCanDoForEach = new ICanDoForEach();

                foreach(var item in iCanDoForEach)
                {

                }
            }
            catch (Exception ex)
            {

                Console.WriteLine(ex.Message);
            }
        }
    }
```
Now, if you try to compile the sample above, you will get the following error
> foreach statement cannot operate on variables of type '....ICanDoForEach' because '...ICanDoForEach' does not contain a public definition for 'GetEnumerator'

Well, no mentioning of the *IEnumerable interface* what so ever!?

As it turns out, not even the *IEnumerator interface* is needed the following code will work and you will be able to use a *foreach* on the class *IcanDoForeach*


```csharp
    class MyOwnEnumerator
    {
        private int counter = 0;
        public object Current { get
            {
                return 17;
            }
        }
        public bool MoveNext()
        {
            counter++;
            return counter <= 5;
        }
    }
    class ICanDoForEach
    {
        public MyOwnEnumerator GetEnumerator()
        {
            return new MyOwnEnumerator();
        }
    }

```

[Back to top](#table-of-content)