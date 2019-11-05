# 插件EmmyLua的介绍



[开源地址][url_github]

[说明文档][url_doc]



- [@class class declaration annotation](https://emmylua.github.io/annotations/class.html)

  ```lua
  ---@class MY_TYPE[:PARENT_TYPE] [@comment]
  ```

- [@type type annotation](https://emmylua.github.io/annotations/type.html)

  ```lua
  ---@type MY_TYPE[|OTHER_TYPE] [@comment]
  ---@type MY_TYPE[] 声明数组
  ---@type table<KEY_TYPE, VALUE_TYPE> 声明table
  ---@type fun(param:MY_TYPE):RETURN_TYPE 声明函数
  
  ```

- [@alias 别名注解](https://emmylua.github.io/annotations/alias.html)

  ```lua
  ---@alias Handler fun(type: string, data: any):void
  
  ---@alias IOEventEnum string | "'onClosed'" | "'onData'" 声明类似枚举
  
  ---@param event IOEventEnum
  ---@param handler Handler | "function(type, data) print(data) end"
  function addEventListener(event, handler)
  end
  
  ---@alias IOEventEnum string | "'onClosed'" | "'onData'"
  ```

- [@param parameter type annotation](https://emmylua.github.io/annotations/param.html)

  ```lua
  ---@param param_name MY_TYPE[|other_type] [@comment]
  ---@param event string | "'onClosed'" | "'onData'" 声明类似枚举
  ```

- [@return function return type annotation](https://emmylua.github.io/annotations/return.html)

  ```
  ---@return MY_TYPE[|OTHER_TYPE] [@comment]
  ```

- [@field field annotation](https://emmylua.github.io/annotations/field.html)

  ```
  ---@field [public|protected|private] field_name FIELD_TYPE[|OTHER_TYPE] [@comment]
  ```

- [@generic generic annotation](https://emmylua.github.io/annotations/generic.html)

  ```lua
  ---@generic T1 [: PARENT_TYPE] [, T2 [: PARENT_TYPE]]
  ```

- [@vararg 不定参数注解](https://emmylua.github.io/annotations/vararg.html)

  ```
  ---@vararg TYPE
  ```

- [@language language injection](https://emmylua.github.io/annotations/language.html)

  ```lua
  ---@language LANGUAGE_ID
  ```

  

- [Full examples](https://emmylua.github.io/annotations/example.html)

  ```lua
  ---@class Transport @parent class
  ---@public field name string
  local transport = {}
  
  function transport:move()end
  
  ---@class Car : Transport @Car extends Transport
  local car = {}
  function car:move()end
  
  ---@class Ship : Transport @Ship extends Transport
  local ship = {}
  
  ---@param type number @parameter type
  ---@return Car|Ship @may return Car or Ship
  local function create(type)
  -- ignored
  end
  
  local obj = create(1)
  ---now you can see completion for obj
  
  ---@type Car
  local obj2
  ---now you can see completion for obj2
  
  local list = { obj, obj2 }
  ---@param v Transport
  for _, v in ipairs(list) do
  ---not you can see completion for v
  end
  ```



















































开源链接

[url_github] : ( https://github.com/EmmyLua )

[url_doc] : ( https://emmylua.github.io/ )





















[url_github]: 