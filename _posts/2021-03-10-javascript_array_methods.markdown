---
layout: post
title:      "JavaScript Array Methods"
date:       2021-03-10 16:43:30 +0000
permalink:  javascript_array_methods
---


You can do a lot with arrays in JavaScript. Arrays come with many methods that can be used to transform, filter and manipulate data. There are a whole host of ways array methods can be useful, but I will focus on just a few.

### Adding and Removing
Starting simple, the following methods allow you to add or remove data from an array. 

* arr.push() – adds items to the end of an array
* arr.pop() – removes an item from the end of an array
* arr.shift() – removes an item from the beginning of an array
* arr.unshift() – adds items to the beginning of an array

Example
```
const colors = ['yellow', 'red', 'blue'];

const addColor = colors.push('brown');
console.log(colors);
// expected output: Array ['yellow', 'red', 'blue', 'brown'];
```

Docs: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push

### Iteration
If you need to perform an operation on each element of an array, forEach() is a great go-to. Here's a simple example:

```
const array = ['hello', 'world'];

array.forEach(element => console.log(element));

output: 
'hello' 
'world'

```

forEach is useful in scenarios where you need to perform a specific action on each item in an array. For example, a user wants to see all businesses listed by category. You could have a function like this:

```
const newArray = []
          newCategoryArray.forEach(b => {
            newArray.push(`
              <h2>${b.attributes.name}</h2>
              <p>${b.attributes.description}</p>
              <a href="${b.attributes.website}" class="btn btn-primary">${b.attributes.website}</a><br><br>
            `)
          })
          $(".category-modal-body").html(newArray);
          $("#category-modal").modal("show");
        }
```
which uses forEach to iterate over the array of business categories and returns a new array inside your html.

Docs: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach

### Mapping an Array
map() calls a given function for each element of the array and returns an array of results from that mapping.

For example, let's say you have a component that you want to return a list of projects and for each of those projects, you want to show the project name. For that scenario you could do something like this:

```
const Projects = (props) => {

  return (
    <div>
      {props.projects.map(project =>
        <div key={project.id}>
          <Link to={`/projects/${project.id}`}>{project.name}</Link>
          <Vote />
        </div>)}
    </div>
  )
}
```

### Summary

There's a lot you can do with arrays and this post only covers a few of the methods available. [The MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) are really helpful for learning the syntax and seeing some examples. 


