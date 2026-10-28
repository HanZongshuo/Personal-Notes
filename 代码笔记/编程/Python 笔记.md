Ctrl + Shift + T 可以找回丢失的 网页
[Python Object Oriented Programming (OOP) - For Beginners](https://www.youtube.com/watch?v=JeznW_7DlB0)
`__init__ `函数的作用，当每次调用这个函数的时候，都会先默认调用一下init内的内容
```python
class Dog:
    def __init__(self,name):
        self.name = name
        print(name)
    
    def bark(self):
        print(f"{self.name} bark!")
    
name = input("Input the name in here:")
d = Dog(name)
d.bark()
```

# Dictionary
```python
student = {'name':'Jonh','age':'24','course':['Math','Compter Science']}

print(student['name']) #可以用来直接访问
print(student.get('phone','Not_Found'))  #当键值对不存在的时候会返回None或者自定义的内容，而不报错

student['phone'] = '123456' #添加键值对
print(student.get('phone','Not_Found'))

student.update({'name' :'Zongshuo','phone':'45685'}) #这个是更新函数
print(student)

age = student.pop('age')
print(age) #删除并拿到该值
```

# 1. Numpy
## 1.1 整体介绍
[Python深度学习：NumPy数组库_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1H84y1g7RE/?spm_id_from=333.337.search-card.all.click&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61)

列表的缺点在于每个元素可以是多种数据类型，比如数字和字符串，但是numpy在同一数组中只有一种：整数型或者浮点型
```python
import micropip
await micropip.install('numpy') 
import numpy as np 
arr1 = np.array([1,2,3])
print(arr1)

arr2 = np.array([1.0,2,3])
print(arr2) #当有一个是浮点类型的时候，整个全部是浮点类型
```
（2）通化定理，当array是整数型的时候，插入浮点数会被截断为整数，同样把整数插入到浮点数中时会升级为浮点数
但是通过下面的方法可以进行转化
```python
import numpy as np 
arr1 = np.array([1,2,3])
print(arr1)
arr2 = arr1.astype(float)
print(arr2)
```
## 1.2 数组的维度
```python
import numpy as np 
arr1 = np.ones(3) #传入形状 (3)
print(arr1)
print(arr1.shape)
arr2 = np.ones((1,3)) #二维数组
print(arr2)
print(arr2.shape)

arr3 = np.ones((1,1,3)) #三维数组
print(arr3)
print(arr3.shape)
```
数组之间不同维度的转化
```python
import numpy as np 
arr1 = np.arange(10)
print(arr1)
#升级为二维数组
arr2 = arr1.reshape((1,10)) #这里写10或者-1都可以，反正是变成二维数组
print(arr2)

#二维数组降级为一维数组
arr3 = np.arange(10).reshape((2,5))
print(arr3)
arr4 = arr3.reshape(10)
print(arr4)
```

把一个数组升维，比如
```
x =  #size(2,3)  两行三列
[[1, 2, 3],
 [4, 5, 6]]
 
 y = np.expand_dims(x,axis = 0) 也就是说在最前面加一个维度，从(2,3) -> (1,2,3)
 [ 
  [[1,2,3],[4,5,6]]
 ]
 
 y = np.expand_dims(x,axis = 1) 中间加维度(2,1,3)
 [
   [[1,2,3]],
   [[4,5,6]]
 ]
 
 y = np.expand_dims(x,axis = -1) 最后加维度(2,3,1)
 [
  [[1],[2],[3]],
  [[4],[5],[6]]
 ]
```
 


## 1.3 数组的创建
使用`np.array`直接创建
```python
import numpy as np 
#1.明确知道怎么数据类型的创建
arr1 = np.array([[1],[2],[3]]) #创建一个列矩阵
print(arr1)

#2. 递增数组的创建
arr2 = np.arange(1,21,2) #从1开始，21结束，步长为2的数组
print(arr2)

#3. 同值数组
arr3 = 3.14 *np.ones(3)
print(arr3)

#4. 随机数组(0-1之间,浮点型)
arr4 = np.random.random(5)
print(arr4)
#0-9之间两行三列随机数组
arr5 = np.random.randint(10,size = (2,3))
print(arr5)
```
## 1.4 矩阵的运算
转置只对矩阵有效，如果是数组的话需要首先转化为矩阵
```python
import numpy as np 
#1.明确知道怎么数据类型的创建
arr1 = np.arange(10).reshape((2,5))
print(arr1)
print(f"转置:\n {arr1.T}")
```

矩阵运算部分：
![[Pasted image 20251109235834.png]]
## 1.5 从数组到张量
np 对应的 torch， array 对应着 tensor, Numpy 的n维数组对应着PyTorch的n阶张量，数组和张量可相互转化

## 1.6 散点图
```python
import numpy as np
import matplotlib.pyplot as plt

x_train = np.array([1.0,2.0])
y_train = np.array([300.0,500.0])

print(f"x_train = {x_train}")
print(f"y_train = {y_train}")

plt.scatter(x_train,y_train,marker = 'x', c = 'r')  #基本语法
# Set the title
plt.title("Housing Prices")
# Set the y-axis label
plt.ylabel('Price (in 1000s of dollars)')
# Set the x-axis label
plt.xlabel('Size (1000 sqft)')
plt.show()
```

## 优化函数
`minimize` 是一个通用的优化函数，用于找到使目标函数最小化的参数值。
```
from scipy.optimize import minimize
result = minimize(fun, x0, args=(), method='')
```
fun为目标函数，比如loss损失函数，x0为初始猜测，可以是数组或者是列表，然后args是其余参数，method指定优化算法：
```
method='Nelder-Mead'  # 单纯形法，不需要梯度
method='BFGS'         # 拟牛顿法，需要梯度
method='L-BFGS-B'     # 有界的 BFGS，可以设置参数范围
method='Powell'       # Powell 方法，不需要梯度
```
返回值解释：
```
result = minimize(l1_loss, initial_params, args=(x, y), method='Nelder-Mead')

# result 是一个 OptimizeResult 对象，包含：
print(result.x)       # 最优参数 [w, b]
print(result.fun)     # 最优函数值（最小损失）
print(result.success) # 是否成功收敛 (True/False)
print(result.message) # 优化状态信息
print(result.nit)     # 迭代次数
```

# FastAPI

## 1. 路由
路由就是URL地址与处理函数之间的映射关系。通过输入地址来映射到一个函数
```
app = FastAPI() #首先有一个实例 
@app.get("/")  #装饰器：实例 + 方法 + 路径
async def root():  
    return {"message": "Hello World"}
```

## 2. 参数传递
1. 路径参数适合获取某个用户ID 的用户信息，或者查询某特定项目
```
@app.get("/users/{user_id}") 
async def read_user(user_id: int): # 自动将 user_id 转为整数并校验        return {"user_id": user_id, "message": "Fetching user data"}
```
访问示例：`http://127.0.0.1:8000/users/1024`

可以通过Path()函数来对路径参数进行限制
```
# 注意导入
from fastapi import FastAPI,Path

...
async def book(id: int = Path(...,gt = 0,lt = 101,description = "It should between 0-101")):
```

2. 查询参数
查询参数是URL中?后面的内容，通常用于过滤，排序和分页。适用于搜索关键字，筛选状态，分页等。
```
@app.get("/news/news_list")  
async def news_list(
	skip:int = Query(0,description = "Skipped items",lt = 100),
	limit: int = 10):  
    return {"skip": skip, "limit": limit}
```
访问示例 `http://127.0.0.1:8000/items/?skip=20&limit=50`

3. 请求体
适用于发送复杂结构化的数据，数据不会显示在URL中,在消息体body中
[08-FastAPI基础入门-请求体参数_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1zV2QBtE39?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=8) 
```
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()
class User(BaseModel):
    name: str = Field(default="Han",min_length= 2, max_length= 10, description="It should be 2 - 10 words")
    password: str = Field(min_length=3,max_length=20)
    
@app.post("/register")  
async def register(user: User):  
    return {"name": user.name, "password": user.password}
```

## 3. 响应类型
FastAPI会把路径操作返回的Python对象，经过jsonable_encoder 转化为JSON兼容格式，包装成为JSONResponse返回，这样就可以在不同环境中交流。

默认情况下的相应类型是json，也可以通过在装饰器中指定响应类来确定，比如HTML,纯文本等。 当然返回响应对象也可以，适用于文件下载，图片，流式响应。

1. 在装饰器中进行指定
```
from fastapi.responses import HTMLResponse

@app.get("/html",response_class=HTMLResponse)  
async def get_html():  
    return "<h1>Hello World<h1>"
```
2. 返回图像
```
from fastapi.responses import FileResponse

@app.get("/file")  #这里不声明的话再return中声明即可
async def get_file():  
    path = "./test.jpg"    #表示在当前路径下
    return FileResponse(path)
```

3. 自定义的类
```
from fastapi import  FastAPI,HTTPException
class News(BaseModel):  
    id : int  
    title : str  
    content : str  
  
@app.get("/news/{id}", response_model=News)  
async def get_news(id: int):  
    id_list = [1,2,3,4,5,6]  
    if id not in id_list:  
        raise HTTPException(status_code=404, detail="Exceed Limit")  # 必须与class中内容完全对应
    return {  
        "id": id,  
        "title":"The title",  
        "content":"The content",  
    }
```
假如不一致的话，就会出现
`{'type': 'missing', 'loc': ('response', 'content'), 'msg': 'Field required', 'input': {'id': 8, 'title': 'The title'}}`
这种报错 


