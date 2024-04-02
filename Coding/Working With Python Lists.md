Python lists are like arrays, but not exactly!
## Creating a List
It's simple dummy: **my_list=[]**
## Looping over a list

Loop over single elem:
```
for elem in my_list:
```

Loop over elem and indexes:
```
for index, value in enumerate(my_list, start=1):
```

Loop using only index:
```
for i in range(0, len(my_list))
```

## Useful List Methods
**my_list.append(elem)**: Add elem to the end of the list

**if elem in my_list**: return true if elem appears in my_list

**my_list.count(elem)**: Count the number of times elem appears in the list

**sort(my_list)**: Sorts the list

