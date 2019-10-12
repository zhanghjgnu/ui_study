## golang的gin框架获取请求参数的方法：
### 1 传入的是json方式的请求：
    接收请求接口: v1.POST("/saveoutreg", saveoutreg)
    saveoutreg函数：
    func saveoutreg(c *gin.Context) {
        toutstr, _ := schdata.Saveoutreg(c, Db, tversion)
        c.String(http.StatusOK, toutstr)
     }

    schdata.Saveoutreg函数：
    func Saveoutreg(c *gin.Context, Db1 *sql.DB, gver string) (string, error) {  
      data, _ := ioutil.ReadAll(c.Request.Body)
      log.Println("ctx0.Request.body:"+string(data))
      ...

    重点就是这行：
    data, _ := ioutil.ReadAll(c.Request.Body)

### 2 传入的是form,参数不固定

    c.Request.ParseForm()
    for k, v := range c.Request.PostForm {
        fmt.Printf("k:%v\n", k)
        fmt.Printf("v:%v\n", v)
    }

### 3 获取请求中的参数
   假如有这么一个请求:
    POST   /post/test?id=1234&page=1  HTTP/1.1
    请求头:  Content-Type: application/x-www-form-urlencoded
    form表单参数:  name=manu&message=this_is_great

    gin的实现:

    id := c.Query("id") //查询请求URL后面的参数
    page := c.DefaultQuery("page", "0") //查询请求URL后面的参数，如果没有填写默认值
    name := c.PostForm("name") //从表单中查询参数
    /////////////////////////////////
    //POST和PUT主体参数优先于URL查询字符串值。
    name := c.Request.FormValue("name") 
    //返回POST并放置body参数，URL查询参数被忽略
    name := c.Request.PostFormValue("name")
    //从表单中查询参数，如果没有填写默认值   
    message := c.DefaultPostForm("message", "aa") 

    假如gin定义的路由路径为:
    router.POST("/post/:uuid", func(c *gin.Context){
        ...
    }

    则获取uuid的值方法为
    uuid := c.Param("uuid") //取得URL中参数

### 4 其他:
    s, _ := c.Get("current_manager") //从用户上下文读取值      
    var u User
    //从http.Request中读取值到User结构体中，手动确定绑定类型binding.Form
    err1 := c.BindWith(&u, binding.Form) 

    //从http.Request中读取值到User结构体中,根据请求方法类型和请求内容格式类型自动确定绑定类型
    err2 := c.Bind(&u)

    //从session中读取值
    //用户上下文和session生命周期不同，每一次请求会生成一个对应的上下文，一次http请求结束，该次请求的上下文结束，一般来说    session(会话)会留存一段时间
    //session(会话)中一般保存用户登录状态等信息，context(上下文)主要用于在一次http请求中，在中间件(流)中进行信息传递
    user := sessions.Default(c).get("user")

    自动绑定结构体：
    if err := c.ShouldBindJSON(&tor); err != nil {
      log.Println("患者姓名=" + tor.PatientName)
      ... ...
    }
