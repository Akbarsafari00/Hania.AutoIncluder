# Hania.AutoIncluder

[![Build status](https://ci.appveyor.com/api/projects/status/q261l3sbokafmx1o/branch/master?svg=true)](https://www.nuget.org/packages/Hania.AutoIncluder/)
[![NuGet](http://img.shields.io/nuget/v/Hania.autoIncluder.svg)](https://www.nuget.org/packages/Hania.AutoIncluder/)
[![Author](https://img.shields.io/badge/Author-Akbar%20Ahmadi%20Saray-brightgreen.svg)](https://www.nuget.org/packages/Hania.AutoIncluder/)
[![Company](https://img.shields.io/badge/Company-Http%3A%2F%2FHaniaGroup.ir-orange.svg)](https://www.nuget.org/packages/Hania.AutoIncluder/)


### What is Hania.AutoIncluder?

Hania.AutoIncluder is a simple little library built to solve a Include Relative Data in Mvc Core EntityFrameWork


### Where can I get it?

First, [install NuGet](http://docs.nuget.org/docs/start-here/installing-nuget). Then, install [Hania.AutoIncluder](https://www.nuget.org/packages/HaniaMapper/) from the package manager console:

```
PM> Install-Package Hania.AutoIncluder 
```


### How do I get started?

#### First, Create Book Model Class 

```csharp
using System.Collections;
using System.Collections.Generic;
using HaniaIncluder.Attributes;

namespace AutoIncluder.Models
{
    public class Book
    {
        public int Id { get; set; }
        public string Descreption { get; set; }
        public string Title { get; set; }
        
        //Important : For Include Most be set [Include] Attribute To Property
        [Include]
        public Author Author  { get; set; }
        public int AuthorId { get; set; }
        
    }
}
```

And Author Model Class


```csharp
using System.Collections;
using System.Collections.Generic;
using HaniaIncluder.Attributes;

namespace AutoIncluder.Models
{
    public class Author
    {
        public int Id { get; set; }
        public string Fullname { get; set; }
        public string Address { get; set; }
        public string Phone { get; set; }
        public int Age { get; set; }
        
        [Include]
        public ICollection<Book> Books { get; set; }
    }
}

```




#### Now You Can use AuthInclude Exeption method To Include All Object
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using AutoIncluder.Data;
using AutoIncluder.Models;
using HaniaIncluder;

namespace AutoIncluder.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class BooksController : ControllerBase
    {
        private readonly ApplicationDbContext _context;

        public BooksController(ApplicationDbContext context)
        {
            _context = context;
        }


        // GET: api/Books
        [HttpGet]
        public IEnumerable<Book> GetBooks()
        {
            var books = _context.Books;
        }
        
        
        // GET: api/Books/AutoInclude
        [HttpGet("AutoInclude")]
        public IEnumerable<Book> GetBooksWithAutoInclude()
        {
            var books = _context.Books.AutoInclude();
        }

       
    }
}

      
``` 

Result On Webs

------------------------Request GET To '/api/Books'-------------------

``` json
[{
    "id":1,
    "descreption":"This Book About c# Programing",
    "title":"C# Programing",
    "author":null,
    "authorId":1
}]
```

-------------------Request GET To '/api/Books/AutoInclude'------------

``` json
[{
    "id":1,
    "descreption":"This Book About c# Programing",
    "title":"C# Programing",
    "author": {
        "id":1,
        "fullname":"Akbar Ahmadi Saray",
        "address":"Iran - WestAzarbaijan - Urmia",
        "phone":"+989371770774",
        "age":22
    },
    "authorId":1
}]

```
Enjoy It :))))


