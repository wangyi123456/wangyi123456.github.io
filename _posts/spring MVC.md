## spring MVC

### 1 执行流程

![image-20210525214654782](E:\nutstore\md\spring MVC.assets\image-20210525214654782.png)

![1](E:\nutstore\md\spring MVC.assets\1.png)

### 2 使用

#### 1 处理请求

##### 1 get

```java
// /students?current=1&limit=20
@RequestMapping(path = "/students", method = RequestMethod.GET)
@ResponseBody
public String getStudents(
        @RequestParam(name = "current", required = false, defaultValue = "1") int current,
        @RequestParam(name = "limit", required = false, defaultValue = "10") int limit) {
    System.out.println(current);
    System.out.println(limit);
    return "some students";
}
```

```java
// /student/123
@RequestMapping(path = "/student/{id}", method = RequestMethod.GET)
@ResponseBody
public String getStudent(@PathVariable("id") int id) {
    System.out.println(id);
    return "a student";
}
```

##### 2 post

```java
// POST请求
@RequestMapping(path = "/student", method = RequestMethod.POST)
@ResponseBody
public String saveStudent(String name, int age) {
    System.out.println(name);
    System.out.println(age);
    return "success";
}
```

#### 2 处理响应

##### 响应HTML数据

```java
@RequestMapping(path = "/teacher", method = RequestMethod.GET)
public ModelAndView getTeacher() {
    ModelAndView mav = new ModelAndView();
    mav.addObject("name", "张三");
    mav.addObject("age", 30);
    mav.setViewName("/demo/view");
    return mav;
}
```

```java
@RequestMapping(path = "/school", method = RequestMethod.GET)
public String getSchool(Model model) {
    model.addAttribute("name", "北京大学");
    model.addAttribute("age", 80);
    return "/demo/view";
}
```

##### 响应JSON数据

```java
@RequestMapping(path = "/emp", method = RequestMethod.GET)
@ResponseBody
public Map<String, Object> getEmp() {
    Map<String, Object> emp = new HashMap<>();
    emp.put("name", "张三");
    emp.put("age", 23);
    emp.put("salary", 8000.00);
    return emp;
}
```

```java
getJSONString封装
    
@RequestMapping(path = "/like", method = RequestMethod.POST)
@ResponseBody
public String like(int entityType, int entityId, int entityUserId) {
    User user = hostHolder.getUser();

    // 点赞
    likeService.like(user.getId(), entityType, entityId, entityUserId);

    // 数量
    long likeCount = likeService.findEntityLikeCount(entityType, entityId);
    // 状态
    int likeStatus = likeService.findEntityLikeStatus(user.getId(), entityType, entityId);
    // 返回的结果
    Map<String, Object> map = new HashMap<>();
    map.put("likeCount", likeCount);
    map.put("likeStatus", likeStatus);

    return CommunityUtil.getJSONString(0, null, map);
}
```

#### 3 处理cookie和session

##### 设置和获得cookie

设置cookie

```java
@RequestMapping(path = "/cookie/set", method = RequestMethod.GET)
@ResponseBody
public String setCookie(HttpServletResponse response) {
    // 创建cookie
    Cookie cookie = new Cookie("code", CommunityUtil.generateUUID());
    // 设置cookie生效的范围
    cookie.setPath("/community/alpha");
    // 设置cookie的生存时间
    cookie.setMaxAge(60 * 10);
    // 发送cookie
    response.addCookie(cookie);

    return "set cookie";
}
```

获得cookie

```java
@RequestMapping(path = "/cookie/get", method = RequestMethod.GET)
@ResponseBody
public String getCookie(@CookieValue("code") String code) {
    System.out.println(code);
    return "get cookie";
}
```

##### 设置和获得session

```java
@RequestMapping(path = "/session/set", method = RequestMethod.GET)
@ResponseBody
public String setSession(HttpSession session) {
    session.setAttribute("id", 1);
    session.setAttribute("name", "Test");
    return "set session";
}

@RequestMapping(path = "/session/get", method = RequestMethod.GET)
@ResponseBody
public String getSession(HttpSession session) {
    System.out.println(session.getAttribute("id"));
    System.out.println(session.getAttribute("name"));
    return "get session";
}
```

#### 4 操作jquery

```java
@RequestMapping(path = "/ajax", method = RequestMethod.POST)
@ResponseBody
public String testAjax(String name, int age) {
    System.out.println(name);
    System.out.println(age);
    return CommunityUtil.getJSONString(0, "操作成功!");
}
```

```HTML
<body>
    <p>
        <input type="button" value="发送" onclick="send();">
    </p>

    <script src="https://code.jquery.com/jquery-3.3.1.min.js" crossorigin="anonymous"></script>
    <script>
        function send() {
            $.post(
                "/community/alpha/ajax",
                {"name":"张三","age":23},
                function(data) {
                    console.log(typeof(data));
                    console.log(data);

                    data = $.parseJSON(data);
                    console.log(typeof(data));
                    console.log(data.code);
                    console.log(data.msg);
                }
            );
        }
    </script>
```

#### 5 创建事务型消息

```java
@Transactional(isolation = Isolation.READ_COMMITTED, propagation = Propagation.REQUIRED)
public Object save1() {
    // 新增用户
    User user = new User();
    user.setUsername("alpha");
    user.setSalt(CommunityUtil.generateUUID().substring(0, 5));
    user.setPassword(CommunityUtil.md5("123" + user.getSalt()));
    user.setEmail("alpha@qq.com");
    user.setHeaderUrl("http://image.nowcoder.com/head/99t.png");
    user.setCreateTime(new Date());
    userMapper.insertUser(user);

    // 新增帖子
    DiscussPost post = new DiscussPost();
    post.setUserId(user.getId());
    post.setTitle("Hello");
    post.setContent("新人报道!");
    post.setCreateTime(new Date());
    discussPostMapper.insertDiscussPost(post);

    Integer.valueOf("abc");

    return "ok";
}

public Object save2() {
    transactionTemplate.setIsolationLevel(TransactionDefinition.ISOLATION_READ_COMMITTED);
    transactionTemplate.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);

    return transactionTemplate.execute(new TransactionCallback<Object>() {
        @Override
        public Object doInTransaction(TransactionStatus status) {
            // 新增用户
            User user = new User();
            user.setUsername("beta");
            user.setSalt(CommunityUtil.generateUUID().substring(0, 5));
            user.setPassword(CommunityUtil.md5("123" + user.getSalt()));
            user.setEmail("beta@qq.com");
            user.setHeaderUrl("http://image.nowcoder.com/head/999t.png");
            user.setCreateTime(new Date());
            userMapper.insertUser(user);

            // 新增帖子
            DiscussPost post = new DiscussPost();
            post.setUserId(user.getId());
            post.setTitle("你好");
            post.setContent("我是新人!");
            post.setCreateTime(new Date());
            discussPostMapper.insertDiscussPost(post);

            Integer.valueOf("abc");

            return "ok";
        }
    });
}
```

#### 6 拦截器

```java
@Component
public class LoginTicketInterceptor implements HandlerInterceptor {

    @Autowired
    private UserService userService;

    @Autowired
    private HostHolder hostHolder;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 从cookie中获取凭证
        String ticket = CookieUtil.getValue(request, "ticket");

        if (ticket != null) {
            // 查询凭证
            LoginTicket loginTicket = userService.findLoginTicket(ticket);
            // 检查凭证是否有效
            if (loginTicket != null && loginTicket.getStatus() == 0 && loginTicket.getExpired().after(new Date())) {
                // 根据凭证查询用户
                User user = userService.findUserById(loginTicket.getUserId());
                // 在本次请求中持有用户
                hostHolder.setUser(user);
            }
        }
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        User user = hostHolder.getUser();
        if (user != null && modelAndView != null) {
            modelAndView.addObject("loginUser", user);
        }
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        hostHolder.clear();
    }
}
```

```java
@Component
public class HostHolder {

    private ThreadLocal<User> users = new ThreadLocal<>();

    public void setUser(User user) {
        users.set(user);
    }

    public User getUser() {
        return users.get();
    }

    public void clear() {
        users.remove();
    }
```

#### 7 统一异常处理，统一记录日志

404 和 500 错误友好显示

500错误拦截所有控制器请求在控制台记录日志

```java
@ControllerAdvice(annotations = Controller.class)
public class ExceptionAdvice {

    private static final Logger logger = LoggerFactory.getLogger(ExceptionAdvice.class);

    @ExceptionHandler({Exception.class})
    public void handleException(Exception e, HttpServletRequest request, HttpServletResponse response) throws IOException {
        logger.error("服务器发生异常: " + e.getMessage());
        for (StackTraceElement element : e.getStackTrace()) {
            logger.error(element.toString());
        }

        String xRequestedWith = request.getHeader("x-requested-with");
        if ("XMLHttpRequest".equals(xRequestedWith)) {
            response.setContentType("application/plain;charset=utf-8");
            PrintWriter writer = response.getWriter();
            writer.write(CommunityUtil.getJSONString(1, "服务器异常!"));
        } else {
            response.sendRedirect(request.getContextPath() + "/error");
        }
    }
```

