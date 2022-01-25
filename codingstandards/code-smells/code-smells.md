# Code Smells

## Regions

Regions are traditionally used to organise a class into areas, allowing the developers to collapse the region in the IDE.

```c#
#region [Public Props]

public void Get(Guid id)
{
    ...
}

public void Save(User user)
{
    ...
}

#endregion
...
```

The code smell here is that the class is probably too large, and should be decomposed into smaller classes that do a single job or collection of related jobs. Smaller classes are much easier to understand, test and refactor.

It also does not enfource the odering of items in the class so a developer could add a public method to an area of the class that does not have a region, negating its use.

### Ways to avoid using regions

#### Group fields, properties and methods in a standard way

- Fields and properties : private to public
- Methods : public, protected, private

#### Decompose large classes into smaller classes with specific jobs

Think about the role of the class, if something does not fit into this class move it to its own class that does that task.


## Defining variables far from where they are used

``` c#
var results = new List<string>();
if (otherValues is null)
{
    return results;
}

foreach (var thing in otherValues)
{
    results.Add(thing.Name);
}
```

The first "if" does not evaluate anything about the results, we are mearly creating an empty array so we can return. 

to improve the readablity we could do this:

```c#
if (otherValues is null)
{
    return Enumerable.Empty<string>();
}

var results = new List<string>();
foreach (var thing in otherValues)
{
    results.Add(thing.Name);
}

// Or

if (otherValues is null)
{
    return Enumerable.Empty<string>();
}

var results = otherValues
    .Select(c => c.Name);
```


## Helper Classes

A helper class is a code smell as it has no defined purpose.

"What does the ProductHelper class do?"

"...It Helps..".

Helper classes become a place to put all code that is related to an area of the system. These files grow quickly as more development is done on the system. This makes it very difficult to a developer to reason about where functionality is and what each method does.

### Alternative

Create specific classes for the methods or related methods. Name the class about what it does. e.g ProductSummaryBuilder or ProductPriceFormatter.

## Avoid the else keyword

May times the else keyword can be avoided by either extracting the code into a function or returning when the if has completed.

### Alternatives

#### Remove when redundant

```c#
// Redundant else
private string GetOutcomeText(bool success)
{
    if (success)
    {
        return "That all worked fine."
    }
    else
    {
        return  "Oh dear, that didn't work at all."
    }
}
// Good
private string GetOutcomeText(bool success)
{
    if (success)
    {
        return "That all worked fine."
    }

    return  "Oh dear, that didn't work at all."
}
```

#### Set a default value

```c#
var outcomeText = "Oh dear, that didn't work at all.";
if (success)
{
    outcomeText = "That all worked fine."
}

return outcomeText
```

#### Early Return

## Immediate defining of a null value

``` c#
string outcomeText = null;
if (success)
{
    outcomeText = "That all worked fine."
}
else
{
    outcomeText = "Oh dear, that didn't work at all."
}
```

``` c#
string outcomeText = success ?
    "That all worked fine." :
    "Oh dear, that didn't work at all.";
```

## Levels of Indentation/Nesting

```c#
public void DoTheLooping(bool success)
{
    if (success)
    {
        for (var i = 0; i < 100; i++)
        {
            if (i > 50)
            {
                foreach(var thing in collection)
                {
                    if(thing.index.equals(i))
                    {
                        // ... more code here
                    }
                }
            }
        }
    }
}
```
### Use methods to break the nesting down

```c#
public void DoTheLooping(bool success)
{
    if (success)
    {
        FindItems()
    }
}

private void FindItems()
{
    for (var i = 0; i < 100; i++)
    {
        if (i > 50)
        {
            findInCollection(i)
        }
    }
}

private void FindInCollection(int index)
{
    foreach(var thing in collection)
    {
        if (thing.index.equals(index))
        {
            // ... more code here
        }
    }
}
```