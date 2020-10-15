javaScript中的拷贝
## concept
- 基本类型和引用类型 
- 内存中的堆栈
## 深拷贝与浅拷贝的区别
---
深拷贝与浅拷贝的区别是对待对象中的引用类型的拷贝时进行的操作不同。浅拷贝只是复制这些引用类型的指针。而深拷贝会拷贝引用类型的值。
## js中实现浅拷贝的方法
1. 利用Object.assign()方法。
  （用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。）
2. 数组可以用Array.slice()和Array.concat()方法
   + Array.prototype.slice([begin[,end]])   (返回一个新数组)
   由begin和end决定的原数组的浅拷贝
   + Array.prototype.concat(value1[,value2[,value..]])用于合并两个或多个数组不会改变原数组，返回一个新数组
    ```
    //使用slice进行浅拷贝
    //返回一个拷贝的值
    function shallowClone( array){
        return array.slice()
    }
    ```
    ```
    function shallowClone(array){
        let copied;
        copied = [].concat(array);
        return copied;
    }
    ```
## js中实现深拷贝的方法
1. 利用JSON.stringify()和JSON.parse()
    ```
    /**
     *如果对象中有函数，JSON序列化会将其解析为undefined；
     *循环引用时无法正确实现深拷贝
     *如果对象中有NaN,Infinity和-Infinity时,会将其解析为null
    */
    function deepClone(obj){
       let json = JSON.stringify(obj);
       let newObj = JSON.parse(json);
       return newObj;
    }
    ```
    保证JSON安全
2. 利用迭代
  ```
   //没有循环引用的情况
   function deepClone(obj){
       if(obj)
       for(let i = 0;i < obj.leng>)
   }
  ```