- **Filter**(show only the data that true)

```javascript
const items = [
	{ name: 'bike', price: 100 },
	{ name: 'moto', price: 100 },	
	{ name: 'car', price: 100 },
]
const items = [
	{ name: 'bike', price: 100 },
	{ name: 'moto', price: 100 },	
	{ name: 'car', price: 100 },
]
const filterItems = items.filter((item) =. {
	return item.price <= 100
})
```
- **Map**(loop the entire array)
```javascript
const items = [
	{ name: 'bike', price: 100 },
	{ name: 'moto', price: 100 },	
	{ name: 'car', price: 100 },
]
const itemNames = items.map((item)=> { 
return item.price 
})
```
- **Find** (check if there is any in there)
```javascript
const items = [
	{ name: 'bike', price: 100 },
	{ name: 'moto', price: 100 },	
	{ name: 'car', price: 100 },
]
const itemNames = items.find((item)=> { 
	return item.name === 'Album' 
})
```
- **FindIndex**(show the index if it found)
```javascript
const bananaIndex = food.findIndex(()=> item.color ==='yello')
%% if it have it show the index %%
bananaIndex -> 4
%% if it doesn't have it show -1 %%

```
- **IndexOf**(show the index if it found with the count of the )
```javascript
const arr = ['banana', 'apple', 'pineapple', 'apple', 'mango'];
const arrOfFruit = arr.indexOf('apple', 1);

console.log("result:", arrOfFruit);
	result: 1
const bananaIndex = food.indexOf('banana', )
%% if it have it show the index %%
bananaIndex -> 4
%% if it doesn't have it show -1 %%


const arrOfFruit = arr.indexOf('apple', 1);
console.log("result:", arrOfFruit);
```
- - **lastIndexOf**(show the index from the last if it found with the count of the )
```javascript
const arr = ['banana', 'apple', 'pineapple', 'apple', 'mango'];
const arrOfFruit = arr.lastIndexOf('apple', 1);

console.log("result:", arrOfFruit);
	result: 1
const bananaIndex = food.LastIv ndexOf('banana', )
%% if it have it show the index %%
bananaIndex -> 4
%% if it doesn't have it show -1 %%


const arrOfFruit = arr.indexOf('apple', 1);
console.log("result:", arrOfFruit);
```
- 
```

- **Some** (check if it have or not and return true if have)

```javascript
const items = [
	{ name: 'bike', price: 100 },
	{ name: 'moto', price: 100 },	
	{ name: 'car', price: 100 },
]
const itemNames = items.some((item)=> { 
return item.name === 'Album' 
})
```
- **ForEach** (same as map, but can put function)
```javascript
 const items = [
	{ name: 'bike', price: 100 },
	{ name: 'moto', price: 100 },	
	{ name: 'car', price: 100 },
]
 items.forEach((item)=> { return item.name === 'Album' })
```
- **Every**(check is every data is in the condition. it need to match all the array )
```javascript
const items = [
	{ name: 'bike', price: 100 },
	{ name: 'moto', price: 100 },	
	{ name: 'car', price: 100 },
]
const itemNames = items.every((item)=> { 
	return item.price <= 100 
})
```
- **Reduce**
```javascript
const items = [
	{ name: 'bike', price: 100 },
	{ name: 'moto', price: 100 },	
	{ name: 'car', price: 100 },
]
const sumTotal = items.reduce((currentTotal, item)=>{
	return item.price + currentTotal
})
```
- **Include**
```javascript
const items = [1,2,3,4,5]
const includesTow = items.includes(2)

```
- **Concat**(Merge array)
```Javascript
const frouts = ['apple','banana','pineapple']
const vegetables = ['potato'','brocoli']
const food = fruits.concat(vegetables);
```
- **Push**(push data/array to array)
```javascript
const fruits = ['apple','banana','pineapple']
const addNewFruit = fruits.push('apple', 'pipneapple')

result : 5
```
- **Unshift**(push data/array to front of array)
```javascript
const fruits = ['apple','banana','pineapple']
const addNewFruit = fruits.unshift('apple', 'pipneapple')
console.log("result:", addNewFruit);
console.log("result:", arr2);
result : 5
result : ['apple', 'pipneapple', 'apple','banana','pineapple']
```
- **Pop**(remove the last element)
```javascript
const arr2 = ['banana', 'apple', 'pineapple', 'apple', 'mango'];
const removeItem = arr.pop();
console.log("result:", arr2);
result: [ 'banana', 'apple', 'pineapple', 'apple' ]
```
- **Shift**(remove item from the first of array)
```javascript
const arr2 = ['banana', 'apple', 'pineapple', 'apple', 'mango'];
const removeItem = arr2.shift();
console.log("result:", arr2);
result: [ 'banana', 'apple', 'pineapple', 'apple' ]
```
- **Join**(add special character between array element)
```javascript
const arr2 = ['banana', 'apple', 'pineapple', 'apple', 'mango'];
const foodStr = arr2.join('  heang ')
console.log("result:", foodStr);

result: banana  heang  apple  heang  pineapple  heang  apple  heang  mango
```
- **Fill**(replace all or start from of the array  )
```javascript
food.fill(element);
food.fill(element, startIndex);
food.fill(element, startIndex, endIndex);
```
- **CopyithIn**(feature that copy the value and add to the front of the array)
```js
food.copyWithIn(element);
food.copyWithIn(element, startIndex);
food.copyWithIn(element, startIndex, endIndex);
```
- **Slice**(method that allow you to slice, what ever you want)
```js

```