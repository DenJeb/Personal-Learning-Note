# FastAPI

> FastAPI 是一个现代、快速（高性能）的 Python Web 框架，用于构建 API。它基于 Python 的类型提示（Type Hints），使用 Starlette 作为 Web 部分，Pydantic 作为数据部分。

## 目录

- [1. 基础入门](#basics)
  - [1.1 第一个 FastAPI 程序](#1-1)
  - [1.2 路由](#1-2)
  - [1.3 响应](#1-3)
  - [1.4 处理异常](#1-4)
  - [1.5 中间件（控制所有请求）](#1-5)
  - [1.6 依赖注入（自动注入）](#1-6)
- [2. 进阶](#advanced)
  - [2.1 ORM](#2-1)

---

## 1. 基础入门 <a id="basics"></a>

### 1.1 第一个 FastAPI 程序 <a id="1-1"></a>

**安装 FastAPI**

```bash
pip install fastapi uvicorn
```

**FastAPI 程序**

```python
from fastapi import FastAPI

# 创建 FastAPI 实例
app = FastAPI()

# 定义路由
@app.get("/")
async def root():
    return {"message": "Hello World"}

# 定义路由参数
@app.get("/user/{name}")
async def say_hello(name: str):
    return {"message": f"Hello {name}"}

# 运行 FastAPI 应用
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="127.0.0.1", port=8000)

# 访问交互式文档（http://127.0.0.1:8000/docs）
```

### 1.2 路由 <a id="1-2"></a>

路由就是 **URL 地址**和**处理函数**的映射关系，它决定了当用户访问某个 URL 时，会调用哪个处理函数。

#### 1.2.1 路由参数

1. 路径参数
   * 位置：URL 路径的一部分
   * 例如：`/user/{user_id}`
   * 作用：指向唯一、特定的资源
   * 代码：
    ```python
    from fastapi import Path

    @app.get("/user/{user_id}")
    async def get_user(user_id: int = Path(gt=0, lt=101, description="1-100之间的用户ID")):
        return {"user_id": user_id}
    ```
    **`Path`：用于声明路径参数的额外信息和校验规则**
    * `gt`/`ge`：大于/大于等于
    * `lt`/`le`：小于/小于等于
    * `min_length`：最小长度
    * `max_length`：最大长度
    * `description`：参数描述
    * `default`：默认值

2. 查询参数
   * 位置：URL ? 之后，用 & 分隔
   * 例如：`/user?name=Den&age=18`
   * 作用：用于过滤、排序、分页等操作
   * 代码：
    ```python
    from fastapi import Query

    @app.get("/user")
    async def get_user(
        name: str = Query(..., description="用户姓名"),
        age: int = Query(18, description="用户年龄，默认18")
    ):
        return {"name": name, "age": age}
    ```
    **`Query`：用于声明查询参数的额外信息和校验规则**
    * `gt`/`ge`：大于/大于等于
    * `lt`/`le`：小于/小于等于
    * `min_length`：最小长度
    * `max_length`：最大长度
    * `description`：参数描述
    * `default`：默认值

3. 请求体参数
   * 位置：HTTP 请求的消息体（Body）中
   * 作用：用于创建、更新资源，携带大量数据（如 JSON）
   * 代码：
    ```python
    from pydantic import BaseModel, Field

    class User(BaseModel):
        username: str = Field(min_length=2, max_length=10, description="用户名(2-10个字符)")
        password: str = Field(min_length=8, max_length=16, description="密码(8-16个字符)")

    @app.post("/register")
    async def register(user: User):
        # 不返回 password 字段，避免密码泄露（也可用 response_model 过滤，见 1.3.2 节）
        return {"username": user.username}
    ```
    **`BaseModel`：定义 Pydantic 数据模型，并进行数据解析与验证**
    * `gt`/`ge`：大于/大于等于
    * `lt`/`le`：小于/小于等于
    * `min_length`：最小长度
    * `max_length`：最大长度
    * `description`：参数描述

    **`Field`：用于为模型字段添加校验规则、描述、默认值等信息**
    * `gt`/`ge`：大于/大于等于
    * `lt`/`le`：小于/小于等于
    * `min_length`：最小长度
    * `max_length`：最大长度
    * `description`：参数描述

### 1.3 响应 <a id="1-3"></a>

#### 1.3.1 响应类型

默认情况下，FastAPI 会自动将路径操作函数返回的 Python 对象转换为 JSON 格式，并包装为 JSONResponse 返回。

|响应类型|用途|示例|
|---|---|---|
|JSONResponse|默认响应类型|`return {"key": "value"}`|
|HTMLResponse|返回 HTML 内容|`return HTMLResponse(html_content)`|
|PlainTextResponse|返回文本内容|`return PlainTextResponse("text")`|
|StreamingResponse|流式响应|`生成器函数返回数据`|
|FileResponse|返回文件下载|`return FileResponse(path)`|
|RedirectResponse|重定向|`return RedirectResponse(url)`|

**响应类型设置**

1. 装饰器中指定响应类型
    * 场景：固定返回类型（HTML、纯文本等）
    * 代码：
    ```python
    from fastapi.responses import HTMLResponse

    @app.get("/html", response_class=HTMLResponse)
    async def get_html():
        return "<h1>Hello World</h1>"
    ```

2. 返回响应对象
    * 场景：文件下载、图片、流式响应
    * 代码：
    ```python
    from fastapi.responses import FileResponse

    @app.get("/file")
    async def get_file():
        file_path = "./files/example.txt"
        return FileResponse(file_path)
    ```

#### 1.3.2 响应模型（response_model）

`response_model` 用于声明响应的数据模型，FastAPI 会根据它进行**字段过滤、数据校验和 OpenAPI 文档生成**。它只决定返回哪些数据，不改变响应类型（默认仍为 JSON）。

* 场景：过滤敏感字段（如密码）、限制返回字段
* 代码：
```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
    gender: str
    password: str

class UserPublic(BaseModel):
    name: str
    age: int
    gender: str

@app.get("/user", response_model=UserPublic)
async def get_user():
    return {
        "name": "Den",
        "age": 18,
        "gender": "男",
        "password": "12345678"
    }
```

（返回结果中不会包含 `password` 字段）

### 1.4 处理异常 <a id="1-4"></a>

当需要中断请求处理并返回错误响应时，可使用 fastapi.HTTPException（常用于 4xx 客户端错误，如资源未找到、认证失败等，也支持其他状态码）。

**`HTTPException`：用于手动抛出 HTTP 异常**
* `status_code`：HTTP 状态码
* `detail`：错误详情
* 代码：
    ```python
    from fastapi import HTTPException

    @app.get("/user/{user_id}")
    async def get_user(user_id: int):
        id_list = range(1, 101)
        if user_id not in id_list:
            raise HTTPException(
                status_code=404,
                detail="用户不存在"
            )

        return {"user_id": user_id}
    ```

**`@app.exception_handler`：注册全局异常处理器，统一自定义异常响应（如自定义异常类型、统一错误格式）。**

```python
from fastapi import Request
from fastapi.responses import JSONResponse

@app.exception_handler(ValueError)
async def value_error_handler(request: Request, exc: ValueError):
    return JSONResponse(status_code=400, content={"detail": str(exc)})
```

### 1.5 中间件（控制所有请求） <a id="1-5"></a>

中间件是一个在每次请求进入 FastAPI 应用时都会被执行的函数，它在请求到达实际路径操作函数之前执行，并且在响应返回给客户端之前再运行一次。

```txt
客户端 -> 中间件 -> 路由处理函数 -> 中间件 -> 客户端
```

中间件可以有多个，执行顺序由代码**自下而上**（例如：2->1->1->2）。

* 代码：
```python
@app.middleware("http")
async def example(request, call_next):
    print("中间件example  start")
    response = await call_next(request)
    print("中间件example  end")
    return response
```

### 1.6 依赖注入（自动注入） <a id="1-6"></a>

依赖注入系统可以共享通用逻辑，避免代码重复编写。
* 依赖项：可重用的组件（函数/类），负责提供某种功能或数据。
* 注入：FastAPI 自动调用依赖项，将结果注入到路径操作函数中。
* `Depends`：用于声明参数的依赖项，FastAPI 会自动调用依赖项，并**将其返回值注入**该参数。

**优点：**
* 代码复用：一次编写，多处使用。
* 解耦：业务逻辑与基础设施代码分离。
* 易测试：可以独立测试依赖项，而无需修改路径操作函数。

代码示例：
```python
from fastapi import Depends, Query

# 依赖项
def get_commons(
    skip: int = Query(0, ge=0),
    limit: int = Query(10, le=100)
):
    return {"skip": skip, "limit": limit}

# 注入依赖项
@app.get("/news/new_list")
async def get_news_list(commons: dict = Depends(get_commons)):
    return commons
```

---

## 2. 进阶 <a id="advanced"></a>

### 2.1 ORM <a id="2-1"></a>

ORM（Object Relational Mapping，对象关系映射）：通过操作 Python 对象来操作数据库，避免直接编写 SQL 语句。

#### 2.1.1 ORM 简介及安装

（待补充）

#### 2.1.2 建表

（待补充）

#### 2.1.3 在路由中使用 ORM

（待补充）

#### 2.1.4 查询数据

（待补充）

#### 2.1.5 条件查询

（待补充）

#### 2.1.6 聚合查询

（待补充）

#### 2.1.7 分页查询

（待补充）

#### 2.1.8 新增数据

（待补充）

#### 2.1.9 更新数据

（待补充）

#### 2.1.10 删除数据

（待补充）