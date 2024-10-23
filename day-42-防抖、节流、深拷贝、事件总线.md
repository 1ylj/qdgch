# day42-防抖、节流、深拷贝、事件总线

## 一、认识防抖和节流

### 1.1.认识防抖debounce函数

- 防抖：是指我们在输入框输入内容的时候只有当停止后输入框的内容才会发送网页请求，若继续输入也只有在停止的时候才会发送请求
- ![image-20241008215116132](C:\Users\86181\AppData\Roaming\Typora\typora-user-images\image-20241008215116132.png)

### 1.2.认识节流

- 节流就是有一个固定的间隔时间，当触发事件到达这个间隔时间时就会自动发送请求即使输入框还在不断输入内容

- ![image-20241009214336384](C:\Users\86181\AppData\Roaming\Typora\typora-user-images\image-20241009214336384.png)



## 二、underscore使用

### 2.1.防抖函数的案例

- 在某个搜索框中输入自己想要搜素的内容

  - 现象：当输入内容停止后会发送请求，继续输入等到停止后会发送请求

  - 基本实现一定要掌握

    - ```
      <body>
      
        <button>按钮</button>
      
        <input type="text">
      
        <script>
      
          function hydebounce(fn, delay) {
            //1.用于记录上一次事件触发的timer
            let timer = null
      
            //2.触发事件时执行的函数
            const _debounce = () => {
              //2.1.如果再次触发（更多次触发）事件，那么取消上一次的事件
              if(timer) clearTimeout(timer)
              //2.2.延迟去执行对应的fn函数（传入的回调函数）
              timer = setTimeout(() => {
                fn()
                timer = null //执行函数之后将timer重新置null
              },delay);
            }
      
            //返回函数
            return _debounce
          }
      
      
      
        </script>
        
        <script>
          // 1.获取input元素
          const inputEl = document.querySelector("input")
      
          // 2.监听input元素的输入
          // let counter = 1
          // inputEl.oninput = function() {
          //   console.log(`发送网络请求${counter++}:`, this.value)
          // }
          
          // 3.防抖处理代码
          // let counter = 1
          // inputEl.oninput = _.debounce(function() {
          //   console.log(`发送网络请求${counter++}:`, this.value)
          // }, 3000)
      
      
          //4.手写防抖函数
          let counter = 1
          inputEl.oninput = hydebounce(function() {
            console.log(`发送网络请求${counter++}:`)
          }, 3000)
      
        </script>
      
      ```

    - 



## 三、防抖函数实现优化

- 封装函数

- ````
  function LJdebounce(fn, delay) {
   let timer = null
   const _debounce = (...args) => {
     if(timer) clearTimeout(timer)
   	timer = setTimeout(() => {
   	 fn.apply(this,args)
   	},delay )
   }
  return _debounce
  }
  ````

- 需要使用防抖函数直接引用这个工具函数即可



## 四、节流函数实现优化

### 4.1.节流函数的基本实现

- ```
  <body>
  
    <input type="text" name="" id="">
  
    <script>
  
      function LJthrottle(fn, interval) {
        let startTime = 0
  
        const _throttle = function() {
          nowTime = new Date().getTime()
  
          const waitTime = interval - (nowTime - startTime)
  
          if( waitTime <= 0 ) {
            fn()
            startTime = nowTime
          }
        }
        return _throttle
      }
  
      
    </script>
  
    <script>
  
      const inputEl = document.querySelector("input")
  
      let count = 1
      inputEl.oninput = LJthrottle(function() {
        console.log(`发送请求${count++}:`, this.value)
      }, 3000)
    </script>
  
  </body>
  ```

  

## 五、深拷贝函数的实现

### 5.1.认识浅拷贝-引用赋值-深拷贝

- 引用赋值

  - ```
    const obj = {
    name: "ylj",
    age: 18
    }
    
    const newobj = obj
    ```

  - 栈内存中直接指向obj对象

- 浅拷贝

  - 只是对第一层进行拷贝,深层的拷贝不会被拷贝

  - ```
    const obj = {
    name: "ylj",
    age: 18,
    friend: {
    	name:"james"
    }
    }
    //浅拷贝方式一：利用剩余函数
    const newobj = {...obj}
    newobj.name = "yls"
    newobj.friend.name = "kobe"
    console.log(obj.name) //ylj
    console.log(obj.friend.name)//james
    
    //浅拷贝方式二
    const obj3 = Object.assign({}, info)
    obj3.name = "crruy"
    obj3.friend.name = "crruy"
    console.log(info.name, info.friend.name) //crruy  james
    
    ```

  - 



### 5.2.自定义深拷贝函数

```
```



## 六、事件总线工具实现

### 6.1.







## 七、额外忘记知识

- for in 一个对象拿到的是key
- for in 一个数组拿到的是索引
