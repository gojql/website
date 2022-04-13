# Getting Started

JQL is a Go package that provides a fast and [simple](#get-a-value) way to get values from a json object.
It has features such as [one line retrieval](#get-a-value), [conditions](#conditions) for conditional queries, [modifiers](#modifiers) to modify the queried response.

## Installing

To start using JQL, install Go and run `go get`:

```sh
$ go get -u github.com/gojql/core
```

This will retrieve the library.


## Dummy json
We will use below json to demonstrate example queries throughout.
```
      json: {
              "name": {
                "first": "Iron",
                "last": "Man"
              },
              "age": 37,
              "children": [
                "Sara",
                "Alex",
                "Jack"
              ],
              "fav.movie": "Avenger",
              "friends": [
                {
                  "first": "Bruce",
                  "last": "Banner",
                  "age": 44,
                  "salary": $100000,
                  "nets": [
                    "ig",
                    "fb",
                    "tw"
                  ]
                },
                {
                  "first": "Doctor",
                  "last": "Strange",
                  "age": 68,
                  "salary": $100000,
                  "nets": [
                    "fb",
                    "tw"
                  ]
                },
                {
                  "first": "Peter",
                  "last": "Parker",
                  "age": 18,
                  "salary": $1000,
                  "nets": [
                    "fb",
                    "tw"
                  ]
                },
                {
                  "first": "Natasha",
                  "last": "Romanoff",
                  "age": 47,
                  "salary": $10000,
                  "nets": [
                    "ig",
                    "tw"
                  ]
                }
              ]
            }
```
## Get a value

Get searches json for the specified path. A path is in dot syntax, such as "name.last" or "age". When the value is found it's returned immediately.

```go
package main

import "github.com/gojql/core"

func main() {
	value := jql.Query(json, "#name.first")
	println(value.String())
}
```

Prints:

```
Iron
```

## Operators

The basic Operators to begin with are

    - # : "Count" or "All"

    - * : any characters

    - ? : any character

## Conditions

Conditional queries can also be used to fetch data from JSON.

    - Syntax
    	- #(...) : first Match
    	- #(...)# : All Matches


Examples :
```
    Query:
    jql.Query(json, "friends.#(first=="Bruce" && last=="Banner").nets")
      
    Prints:
    ["ig", "fb", "tw"]
```

```  
    Query:
    jql.Query(json, "friends.#(first=="Bruce").age")
      
    Prints:
    44
```

```  
    Query:
    jql.Query(json, "friends.#(age>45).last")
      
    Prints:
    Strange
```


```  
    Query:
    jql.Query(json, "friends.#(age>45)#.first")
      
    Prints:
    ["Doctor", "Natasha"]
```

```  
    Query:
    jql.Query(json, "friends.#(age>45)#.first")
      
    Prints:
    ["Doctor", "Natasha"]
```

```  
    Query:
    jql.Query(json, "friends.#(first=="Bruce")")
      
    Prints:
    {
      "first": "Bruce",
      "last": "Banner",
      "age": 44,
      "salary": 12345,
      "nets": [
        "ig",
        "fb",
        "tw"
      ]
    }
```

```  
    Query:
    jql.Query(json, "friends.#(last!="Banner")# ")
      
    Prints:
    [
      {
        "first": "Doctor",
        "last": "Strange",
        "age": 68,
        "salary": 12345,
        "nets": [
          "fb",
          "tw"
        ]
      },
      {
        "first": "Peter",
        "last": "Parker",
        "age": 18,
        "salary": 12345,
        "nets": [
          "fb",
          "tw"
        ]
      },
      {
        "first": "Natasha",
        "last": "Romanoff",
        "age": 47,
        "salary": 12345,
        "nets": [
          "ig",
          "tw"
        ]
      }
    ]
```

## Modifiers

Queried values from JSON can also be further modified to suit our needs, basic modifiers are

    - Syntax
    	- @<modifier>

    - modifiers
    	- uppercase
    	- lowercase
    	- reverse
    	- sort

Example
```  
    Query:
    jql.Query(json, "friends.#(age>45)#.first.@uppercase")
      
    Prints:
    ["DOCTOR", "NATASHA"]
```