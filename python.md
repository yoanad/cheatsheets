# Python cheatsheet

## Arrays
`cars = ["Ford", "Volvo", "BMW"]
x = len(cars)
# append to array
cars.append("Honda")
# add to position
cars.insert()
# remove item 
cars.pop(1)`

## For loops
colors = ["red", "green", "blue", "purple"]
for i in range(len(colors)):
    print(colors[i])

colors = ["red", "green", "blue", "purple"]
for color in colors:
    print(color)
    
presidents = ["Washington", "Adams", "Jefferson", "Madison", "Monroe", "Adams", "Jackson"]
for num, name in enumerate(presidents, start=1):
    print("President {}: {}".format(num, name))
