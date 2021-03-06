In order to keep the code consistent, we follow certain conventions. Many of the choices we have made are somewhat arbitrary and could easily have gone another way. At this point, most of these conventions are already well-established, so please don't re-open a discussion about them unless you have new issues to present.

Notice that these standards are all stylistic. Do not write standards that tell people how to program. For example, we don't need a standard that tells us to always dispose of disposable objects because that's part of the normal "standard" for C# programming. In other words, these standards are about relatively trivial things that we have all agreed to do the same way.

### Making Changes

If you do want to make changes, please don't just edit the wiki. Initiate a discussion of what you want to change on the developer list first. If you add an entire new section, you can edit first and then present it to the list for discussion. However, if you are intending to standardize more things than we usually standardize, it's wise to discuss it first to avoid wasting time!

### General - Please Read This!

Follow these guidelines unless you have an extremely good reason not to. Add a comment explaining why you are not following them so others will know your reasoning.

Don't make arbitrary changes in existing code merely to conform them to these guidelines. Normally, you should only change the parts of a file that you have to edit in order to fix a bug or implement new functionality. In particular, don't use automatic formatting to change an entire file at once as this makes it difficult to identify the underlying code changes when we do a code review.

In cases where we make broad changes in layout or naming, they should be committed separately from any bug fixes or feature changes in order to keep the review process as simple as possible. That said, we don't do this very often, since we have real work to do!

Visual Studio can be set up to match the coding standards by importing the [nunit.vssettings](https://github.com/nunit/docs/blob/master/nunit.vssettings) file from this repository. This file will only change the C# indentation and formatting settings. It will not modify any other Visual Studio settings. It can be imported into Visual Studio 2010 and later by going to Tools | Import and Export Settings...

### Copyright

NUnit is licensed under the MIT / X11 license. Each file is prefixed by the [[NUnit Copyright Notice]] enclosed in appropriate comment characters for the language of the file.

##### Notes:

1. Charlie Poole is the copyright holder for the NUnit code on behalf of the NUnit community, at least until some other arrangement is made. Do not place your name on the copyright line if you wish to contribute code.

2. The year given is the year of the file's creation. Subsequently, as copyrightable (non-trivial) changes are made, additional years or ranges of years may be added. For example, some file might include the statement
   ```
                 Copyright (c) 2007-2015 Charlie Poole
   ```

3. Do not update the copyright years when no changes or only trivial changes are made to a file.

### Language Level

In coding for NUnit, we limit ourselves to C# 3.0 features. We want to keep the code buildable by folks who don't necessarily have the latest compilers.

### Layout

#### Namespace, Class, Structure, Interface, Enumeration and Method Definitions

Place the opening and closing braces on a line by themselves and at the same level of indentation as their parent.

```C#
public class MyClass : BaseClass, SomeInterface
{
    public void SomeMethod(int num, string name)
    {
        // code of the method
    }
}
```

An exception may be made if a method body or class definition is empty

```C#
public virtual void SomeMethod(int num, string name) { }
```

```C#
public class GadgetList : List<Gadget> { }
```

#### Property Definitions

Prefer automatic backing variables wherever possible

```C#
public string SomeProperty { get; private set; }
```

If a getter or setter has only one statement, a single line should normally be used

```C#
public string SomeProperty 
{
    get { return _innerList.SomeProperty; }
}
```

If there is more than one statement, use the same layout as for method definitions

```C#
public string SomeProperty
{
    get
    {
        if (_innerList == null)
            InitializeInnerList();

        return _innerList.SomeProperty;
    }
}
```

#### Spaces

Method declarations and method calls should not have spaces between the method name and the parenthesis, nor within the parenthesis. Put a space after a comma between parameters.

```C#
public void SomeMethod(int x, int y)
{
    Console.WriteLine("{0}+{1}={2}", x, y, x+y );
}
```

Control flow statements should have a space between the keyword and the parenthesis, but not within the parenthesis.

```C#
for (int i = 1; i < 10; i++)
{
    // Do Something
}
```

There should be no spaces in expression parenthesis, type casts, generics or array brackets, but there should be a space before and after binary operators.

```C#
int x = a * (b + c);
var list = new List<int>();
list.Add(x);
var y = (double)list[0];
```

#### Indentation

Use four consecutive spaces per level of indent. Don't use tabs - except where the IDE can be set to convert tabs to four spaces. In Visual Studio, set the tab size to 4, the indent size to 4 and make sure Insert spaces is selected.

Indent content of code blocks.

In switch statements, indent both the case labels and the case blocks. Indent case blocks even if not using braces. 

```C#
switch (name)
{
    case "John":
        break;
}
```

### Newlines

Methods and Properties should be separated by one blank line. Private member variables should have no blank lines.

Blocks of related code should have not have any blank lines. Blank lines can be used to visually group sections of code, but their should never be multiple blank lines.

If brackets are not used on a control flow statement with a single line, a blank line should follow.

```C#
public static double GetAttribute(XmlNode result, string name, double defaultValue)
{
    var attr = result.Attributes[name];

    double attributeValue;
    if (attr == null || !double.TryParse(attr.Value, NumberStyles.Any, CultureInfo.InvariantCulture, out attributeValue))
        return defaultValue;

    return attributeValue;
}
```

### Naming

The following table shows our naming standard for various types of names. All names should clear enough that somebody unfamiliar with the code can learn about the code by reading them, rather than having to understand the code in order to figure out the names. We don't use any form of "Hungarian" notation.

<table>
<tr><th>Named Item</th><th>Naming Standard</th><th>Notes</th></tr>
<tr><td>Namespaces</td><td>PascalCasing</td><td></td></tr>
<tr><td>Types</td><td>PascalCasing</td><td>For framework types with a special C# name, we use the C# name. So... <code>int</code> rather than <code>System.Int32</code>.</td></tr>
<tr><td>Methods</td><td>PascalCasing</td><td></td></tr>
<tr><td>Properties</td><td>PascalCasing</td><td></td></tr>
<tr><td>Public Fields</td><td>PascalCasing</td><td>But these should be avoided</td></tr>
<tr><td>Private, Protected and Internal Fields</td><td>_camelCasing</td><td>Do not use <code>this</code> with fields designated by a leading underscore. <b>Note that our old standard did not use the underscore.</b> Keep each file to the same standard, renaming when changes are made.</td></tr>
<tr><td>Parameters</td><td>camelCasing</td><td></td></tr>
<tr><td>Local Variables</td><td>camelCasing</td><td></td></tr>
</table>

### Comments

Use doc comments on all publicly accessible members. Keep the audience in mind. For example, comments on publicly used framework methods or attributes should be written for easy understanding by users, while comments on internal methods should target folks who work on NUnit.

Don't comment what is obvious.

Do comment unusual algorithms and the reasoning behind some choices made.

Use TODO comments when needed, but make sure to go back periodically and do whatever it is!

### File Organization

Normally, have one public type per source file. An exception is made for a simple enumeration, which is used in the interface of the public type and seems to "belong" to it. Example: TestResult and ResultState

Name the source file after the public type it represents.

The Directory hierarchy and Namespace hierarchy should match. For example, if the root namespace for a project is `NUnit.Framework`, files in the Constraints subdirectory should be in the `NUnit.Framework.Constraints` namespace.

Wherever possible, classes should be laid out in the following order,

1. Private member variables
2. Constructors
3. Dispose
4. Public Properties
5. Protected Properties
6. Private Properties
7. Public Methods
8. Protected Methods
9. Private Methods

Using statements should be sorted as follows: 
  * All System namespaces
  * All Other namespaces, including NUnit's

It is permissible, but not required, to place using statements inside the namespace block, in shortened form, for namespaces that are descendents of the namespace itself. Note that the compiler will permit other uses of shortened namespaces within the namespace block, but we prefer to limit ourselves to descendants. Non-descendant namespaces should be listed in full form in the main using block.

```C#
using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using NUnit.Engine.Interfaces;
using NUnit.Engine.Internal.Execution; // OK, but inside the namespace is preferred

namespace NUnit.Engine.Internal
{
    using Execution; // Preferred location
    ...
}
```

### Use of Regions

[Needs to be filled in]

### Use of the var keyword

The `var` keyword should be used where the type is obvious to someone reading the code, for example when creating a new object. Use the full type whenever the type is not obvious, for example when initializing a variable with the return value of a method.

```C#
var i = 12;
var list = new List<int>();
Foo foo = GetFoo();
``` 