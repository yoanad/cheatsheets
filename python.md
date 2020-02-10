# Python cheatsheet
- *List* is a collection which is ordered and changeable. - Allows duplicate members.
- *Tuple* is a collection which is ordered and unchangeable. Allows duplicate members.
- *Set* is a collection which is unordered and unindexed. No duplicate members.
- *Dictionary* is a collection which is unordered, changeable and indexed. No duplicate members.

## Arrays (lists)
```cars = ["Ford", "Volvo", "BMW"]
x = len(cars)
```
- append to array
``cars.append("Honda")``
- add to position
``cars.insert()``
- remove item 
``cars.pop(1)``
- get item
``thislist[1]``
- get last item
``thislist[-1]``
- range of indexes
``thislist[2:5]``

The search will start at index 2 (included) and end at index 5 (not included).

``thislist[2:]``

From 2 ot end
- range of negative indexes
``thislist[-4:-1]``

from index -4 (included) to index -1 (excluded)
- check if item exists
``if "apple" in thislist:``
- join two lists
``list3 = list1 + list2``
- remove last item
``list.pop()``

## For loops
```
colors = ["red", "green", "blue", "purple"]
for i in range(len(colors)):
    print(colors[i])
```

```colors = ["red", "green", "blue", "purple"]
for color in colors:
    print(color)
```
```  
presidents = ["Washington", "Adams", "Jefferson", "Madison", "Monroe", "Adams", "Jackson"]
for num, name in enumerate(presidents, start=1):
    print("President {}: {}".format(num, name))
```

## Sets
```thisset = {"apple", "banana", "cherry"}

for x in thisset:
  print(x)
```
- add to set
``thisset.add("orange")``
- add multiple items 
``thisset.update(["orange", "mango", "grapes"])``
- length
``len(thisset)``
- remove
``thisset.remove("banana")``
- check if in set
``print("banana" in thisset)``
- remove last item
``thisset.pop()``
- set from array
``s = set(lst)``

## Dictionary 
```thisdict = {
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964
}
```
- access items
``x = thisdict["model"]``
or
``x = thisdict.get("model")``
- change val
``thisdict["year"] = 2018``
- loop
Print all key names
```for x in thisdict:
  print(x)
```
- print all values
```for x in thisdict:
  print(thisdict[x])
```
- return the values
```for x in thisdict.values():
  print(x)
```
- Loop through both keys and values
```for x, y in thisdict.items():
  print(x, y)
```
- check if key exists
```if "model" in thisdict:
  print("Yes, 'model' is one of the keys in the thisdict dictionary")
```
- adding items
``thisdict["color"] = "red"``
- remove item
``thisdict.pop("model")``
- remove last inserted item
``thisdict.popitem()``
- empty dict
``thisdict.clear()``
- copy a dict
``mydict = thisdict.copy()``
