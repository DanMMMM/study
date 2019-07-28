### 数组常用方法总结
ECMAScript中的Array类型每一项可以保存任何类型的数据，而且Array类型的大小可以动态调整，可以随着数据的添加自动增长。
#### 创建数组
>var arr = new Array('数组长度|数组元素');
#### 检测数组
>Array.isArray(arr)
#### 转换为string
>arr.toString()  
>arr.toLocalString()  
>arr.valueOf()
#### 添加删除方法
>Array.push('元素1','元素2',...'元素n') 在数组结末尾插入元素并返回修改后数组的长度  
>Array.pop() 删除数组末尾的元素并返回删除的元素    
>Array.unshift('元素1','元素2',...'元素n') 在数据前端添加元素并返回数组长度  
>Array.shift() 删除数组的第一项并返回删除的元素
#### 排序方法
>arr.sort('排序函数') 数组调用toString()方法后比较每一项字符串  
>arr.reverse() 反转数组的位置
#### 操作方法
>arr.concat('元素1','元素2',...'元素n') 将arr数组与参数合并成一个新数组，不影响原数组  
>arr.slice(start,end) 获取arr数组从start到end（不包括end位置）的数据，不影响原数组  
>arr.splice(index, num, '元素1','元素2',...'元素n') 指定index和num能删除数组中从index开始的num个数，将num设置为0可以在index为位置添加数据，将num设置为任意数值可以删除num个数再插入数据，方法返回从数组中删除的数据  
>arr.join('分隔符') 将arr数组以分隔符连接成一个字符串  
#### 位置方法
>arr.indexof('要查找的数据','开始查找的位置') 从开始查找的位置查找数据并返回数据所在位置的下标，找不到返回-1，使用全等操作符查找  
>arr.lastIndexOf('要查找的数据','开始查找的位置') 从后往前查找
### 迭代方法
>arr.every(function(item, index, array)) 每一项都返回true，方法返回true  
>arr.some(function(item, index, array)) 某一项返回true，方法返回true  
>arr.filter(function(item, index, array)) 返回函数中会返回true的项组成的数组  
>arr.map(function(item, index, array)) 返回函数调用的结果组成的函数  
>arr.forEach(function(item, index, array)) 等同于使用for循环迭代数组，改变原数组，没有返回值
### 归并方法
>arr.reduce(function(prev, cur, index, array)) 迭代数组所有项，然后返回最终值，每次迭代过程中返回的值都会作为第一个参数传给下一项  
>arr.reduceRight(function(prev, cur, index, array)) 与reduce方向相反