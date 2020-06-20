# 一图解释 map / reduce / filter

> map, filter, and reduce explained with emoji 😂

<img src="https://camo.githubusercontent.com/58f08c75fb388dea2b6eba00f41a8d98e5f3647d/68747470733a2f2f692e726564642e69742f796637727733706a696170782e6a7067" alt="map-filter-reduce-in-emoji-1.png" style="zoom:50%;" />

函数式编程思想是 JavaScript 语言避其自身固有劣势的一个极佳工具或方法。map reduce filter 等方法是其典型。

## map

map 是对单个材料的加工，最后返回对应的成品列表。比如牛肉制成肯德基双层香辣汉堡、红薯制成脆香原味薯条、鸡肉裹上面粉炸成外酥里内的鸡腿、玉米制成劲爆爆米花。

```javascript
['🐮', '🍠', '🐔', '🌽'].map(cook)
// => ["🍔", "🍿", "🍗", "🍿"]

function cook(ingredient) {
  switch (ingredient) {
    case '🐮':
			return '🍔';
    case '🍠':
			return '🍟';
    case '🐔':
			return '🍗';
    case '🌽':
			return '🍿';
		default:
			return '';
  }
}
```

## filter

filter 是过滤，只保留符合条件的产品，比如只保留符合素食主义的食物：薯条和爆米花。

```javascript
['🍔', '🍟', '🍗', '🍿'].filter(isVegetarian)
// => ['🍟', '🍿']

function isVegetarian(ingredient) {
  const VEGETABLE_LIST = ['🍟', '🍿'];
  
  return VEGETABLE_LIST.includes(ingredient);
}
```

## reduce

reduce 是多种食物经消化吸收过后变成一种产物。比如一顿饭你点了一个肯德基全家桶，最后出来的只能是便便。

```javascript
['🍔', '🍟', '🍗', '🍿'].reduce(eat)
// => '💩'

function eat(ingredient) {
	return '💩'
}
```

>Picture showing three functions: 
>
>1. the map function taking an array containing the cow face, roasted sweet potato, chicken, and ear of maize emojis and the cook function as its arguments and returning an array containing the cheeseburger, french fries, poultry legs and popcorn emojis;
>2. the filter function taking an array containing the cheeseburger, french fries, poultry legs and popcorn emojis and the isVegetarian functions as its arguments and returning an array containing the french fries and popcorn emojis;
>3. The reduce function taking an array containing the cheeseburger, french fries, poultry legs and popcorn emojis and the eat function as its arguments and returning the pile of poo emoji as its result.
