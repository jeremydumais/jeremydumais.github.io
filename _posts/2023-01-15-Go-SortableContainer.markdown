---
layout: post
title:  "Go : Creating a sortable custom container"
date:   2023-01-15 20:20:00 -0400
categories: go
---

In this blog post we will create a sortable custom container using the
[Interface](https://pkg.go.dev/sort#Interface) from the standard library sort 
module.

Let's first take a look at the sort interface from the standard library.

```go
type Interface interface {
	// Len is the number of elements in the collection.
	Len() int

	// Less reports whether the element with index i
	// must sort before the element with index j.
	//
	// If both Less(i, j) and Less(j, i) are false,
	// then the elements at index i and j are considered equal.
	// Sort may place equal elements in any order in the final result,
	// while Stable preserves the original input order of equal elements.
	//
	// Less must describe a transitive ordering:
	//  - if both Less(i, j) and Less(j, k) are true, then Less(i, k) must be true as well.
	//  - if both Less(i, j) and Less(j, k) are false, then Less(i, k) must be false as well.
	//
	// Note that floating-point comparison (the < operator on float32 or float64 values)
	// is not a transitive ordering when not-a-number (NaN) values are involved.
	// See Float64Slice.Less for a correct implementation for floating-point values.
	Less(i, j int) bool

	// Swap swaps the elements with indexes i and j.
	Swap(i, j int)
}
```

It's pretty easy that way, considering that we only need to implement 3 methods
in our struct to then use the sort function of the sort module.

## Our demo implementation

For the purpose of the demo, we will create a custom container. Let's use a
simple one, the Single Linked List.

Here's an implementation example where I kept to the minimum for readability:

```go
// Node This represent a node item in the list.
type Node struct {
	Next  *Node
	Value string
}

// SingleLinkedList is the interface to the single linked list structure. 
// A single linked list is a type of linked list in which each node has only 
// one link to the next node in the list.
type SingleLinkedList interface {
    // Return the first node of the list. If the list is empty nil will be returned.
    Head() *Node

    // Return the number of nodes in the list.
    Len() int

    // Add a new node at the end of the list.
    AddNode(value string)

    // Less reports whether the element with index i
	// must sort before the element with index j.
	//
	// If both Less(i, j) and Less(j, i) are false,
	// then the elements at index i and j are considered equal.
	// Sort may place equal elements in any order in the final result,
	// while Stable preserves the original input order of equal elements.
	//
	// Less must describe a transitive ordering:
	//  - if both Less(i, j) and Less(j, k) are true, then Less(i, k) must be true as well.
	//  - if both Less(i, j) and Less(j, k) are false, then Less(i, k) must be false as well.
    Less(i, j int) bool

    // Swap swaps the elements with indexes i and j.
	Swap(i, j int)
}

// Single linked list: a type of linked list in which each node has only one
// link to the next node in the list.
type singleLinkedList struct {
	head *Node
    nodeCount int
}

// MakeSingleLinkedList is the constructor of the SingleLinkedList struct.
func MakeSingleLinkedList() SingleLinkedList {
    retval := &singleLinkedList{ nil, 0 }
    return retval 
}

func (list *singleLinkedList) Head() *Node {
    return list.head
}

func (list *singleLinkedList) Len() int {
    return list.nodeCount
}

func (list *singleLinkedList) AddNode(value string) {
    newNode := &Node{nil, value}
    if list.head == nil {
        list.head = newNode
    } else {
        // Go to last node
        lastNode := list.head
        for (lastNode.Next != nil) {
            lastNode = lastNode.Next
        }
        lastNode.Next = newNode
    }
    list.nodeCount++
}

func (list *singleLinkedList) Less(i, j int) bool {
    nodeI, err := list.fetchNode(i)
    if err != nil {
        return false
    }
    nodeJ, err := list.fetchNode(j)
    if err != nil {
        return false
    }
    return nodeI.Value < nodeJ.Value
}

func (list *singleLinkedList) Swap(i, j int) {
    nodeI, err := list.fetchNode(i)
    if err != nil {
        return
    }
    nodeJ, err := list.fetchNode(j)
    if err != nil {
        return
    }
    temp := nodeJ.Value
    nodeJ.Value = nodeI.Value
    nodeI.Value = temp
}

func (list *singleLinkedList) fetchNode(index int) (*Node, error) {
    if index < 0 || index > list.nodeCount - 1 {
        return nil, errors.New("index out of range") 
    }
    currentNode := list.head
    for i := 0; i < index; i++ {
        currentNode = currentNode.Next
    }
    return currentNode, nil
}

```

Briefly, our custom container here exposes the Head method to get the first
element of the list. We can use the Next pointer of the Node struct to navigate
to the Next element and the AddNode method to add a new element to the list.

## The sort function

With the Len, Less and Swap implemented, we can then use the sort function of the
sort module.

Here's an example of a very small program that sort a SingleLinkedList:

```go
package main

import (
    "fmt"
    "sort"
)

// Put your SingleLinkedList implementation here

func main() {
    list := MakeSingleLinkedList()
    list.AddNode("Test")
    list.AddNode("ABC")
    list.AddNode("123")
    sort.Sort(list)
    var currentNode *Node
    for i := 0; i < list.Len(); i++ {
        if i == 0 {
            currentNode = list.Head()
        } else {
            currentNode = currentNode.Next
        }
        fmt.Println(currentNode.Value)
    }
}
```

Output:

```bash
123
ABC
Test
```

## Use other predefined functions

Once the Interface is implemented you can use more functions like the reverse
function to reverse the order of the data:

```go
package main

import (
    "fmt"
    "sort"
)

// Put your SingleLinkedList implementation here

func main() {
    list := MakeSingleLinkedList()
    list.AddNode("Test")
    list.AddNode("ABC")
    list.AddNode("123")
    sort.Sort(sort.Reverse(list))
    var currentNode *Node
    for i := 0; i < list.Len(); i++ {
        if i == 0 {
            currentNode = list.Head()
        } else {
            currentNode = currentNode.Next
        }
        fmt.Println(currentNode.Value)
    }
}
```

Output:

```bash
Test
ABC
123
```

## Conclusion

Sometimes it's easier to only implement a few methods of an interface to then
be able to use functions from the standard library like we did in this post.

There are plenty of other interfaces that are worth looking:

To create a custom error type:
```go
type error interface {
	Error() string
}
```
<br>
Package math/rand

To be able to generate pseudo-random numbers
```go
type Source64 interface {
	Source
	Uint64() uint64
}
```

<br>
Package fmt

Used to print values passed as an operand to any format that accepts a string 
or to an unformatted printer such as Print. 
```go
type Stringer interface {
	String() string
}
```

...and so much more.
