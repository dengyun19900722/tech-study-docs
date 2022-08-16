### 1.Socket编程

​		以下实例演示了如何实现客户端发送消息到服务器，服务器接收到消息并读取输出，然后写出到客户端客户端接收到输出。

#### 1、建立服务器端

- 服务器建立通信ServerSocket
- 服务器建立Socket接收客户端连接
- 建立IO输入流读取客户端发送的数据
- 建立IO输出流向客户端发送数据消息

服务器端代码:

###### Server.java 文件

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;
 
public class Server {
   public static void main(String[] args) {
      try {
         ServerSocket ss = new ServerSocket(8888);
         System.out.println("启动服务器....");
         Socket s = ss.accept();
         System.out.println("客户端:"+s.getInetAddress().getLocalHost()+"已连接到服务器");
         
         BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
         //读取客户端发送来的消息
         String mess = br.readLine();
         System.out.println("客户端："+mess);
         BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
         bw.write(mess+"\n");
         bw.flush();
      } catch (IOException e) {
         e.printStackTrace();
      }
   }
}
```



以上代码运行输出结果为：

```
启动服务器....
```

#### 2、建立客户端

- 创建Socket通信，设置通信服务器的IP和Port
- 建立IO输出流向服务器发送数据消息
- 建立IO输入流读取服务器发送来的数据消息

客户端代码:

###### Client.java 文件

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.net.Socket;
import java.net.UnknownHostException;
 
public class Client {
   public static void main(String[] args) {
      try {
         Socket s = new Socket("127.0.0.1",8888);
         
         //构建IO
         InputStream is = s.getInputStream();
         OutputStream os = s.getOutputStream();
         
         BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(os));
         //向服务器端发送一条消息
         bw.write("测试客户端和服务器通信，服务器接收到消息返回到客户端\n");
         bw.flush();
         
         //读取服务器返回的消息
         BufferedReader br = new BufferedReader(new InputStreamReader(is));
         String mess = br.readLine();
         System.out.println("服务器："+mess);
      } catch (UnknownHostException e) {
         e.printStackTrace();
      } catch (IOException e) {
         e.printStackTrace();
      }
   }
}
```

以上代码运行输出结果为：

```
服务器：测试客户端和服务器通信，服务器接收到消息返回到客户端

sudo-sh[root@myhost]# systemctl status systemd-hostnamed
vim /etc/fstab 


roles/bootstrap-os/tasks/main.yml


- name: Assign inventory name to unconfigured hostnames (CoreOS, non-Flatcar, Suse and ClearLinux only)
  command: "hostnamectl set-hostname {{ inventory_hostname }}"



```



### 2. Java8

#### 2.1 Date

##### 2.1.1 

https://www.concretepage.com/java/java-8/java-localdate-to-instant-timestamp

#### 2.2 list

##### 2.2.1 list排序、去重

1、需求：根据对象属性字段给list集合去重；

2、样例：

```java
package com.zyt.controller;

import java.util.*;
import java.util.stream.Collectors;

/**
 * 根据对象属性字段给list集合去重
 *
 * @author Lance
 * @date 2017/03/14
 */
public class ListQC {
  public static void main(String[] args) {
      List<User> userList = new ArrayList<User>();
      User user1 = new User("AAA", "001");
      userList.add(user1);
      User user2 = new User("AAA", "002");
      userList.add(user2);
      User user3 = new User("AAB", "002");
      userList.add(user3);
      User user4 = new User("AAB", "003");
      userList.add(user4);
      User user5 = new User("AAB", "004");
      userList.add(user5);
      for (User u : userList) {
          System.out.println(u.getName()+"-----"+u.getUserId());
      }
      System.out.println("去重后=======>");
      List<User> userListNoDupAndSort = removeDuplicateUserId(userList);
      for (User u : userListNoDupAndSort) {
          System.out.println(u.getName()+"-----"+u.getUserId());
      }
  }

    private static List<User> removeDuplicateUserId(List<User> users) {
        Set<String> nameSet = new HashSet<>();
        List<User> removeDuplicateUserList = users.stream().
                //按userId降序排列
                sorted(Comparator.comparing(User::getUserId).reversed()).
                    //通过set对集合去重
                    filter(e -> nameSet.add(e.getName())).
                        //生成最终的集合
                        collect(Collectors.toList());
        return removeDuplicateUserList;
    }
}

class User {
    private String name;
    private String userId;
    public User(String name, String userId) {
        this.name = name;
        this.userId = userId;
    }
    public String getName() {
        return name;
     }
    public void setName(String name) {
         this.name = name;
    }
    public String getUserId() {
        return userId;
    }
    public void setUserId(String userId) {
        this.userId = userId;
    }
}
```

### 3. Java常见数据结构处理

#### 3.1 fastjson

##### 3.1.1 JsonObject、JsonArray和JsonStr和JavaBean之间的互相转化

1. JSONObject

   ![img](https://upload-images.jianshu.io/upload_images/1935339-2f37917f20d5bce2.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

2. JSONArray

   ![img](https://upload-images.jianshu.io/upload_images/1935339-78985353484ffaa4.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

3. 

### 4. springboot体系 

#### 4.1 springboot + 拦截器 + 注解 实现自定义权限验证

https://blog.csdn.net/u012852374/article/details/78601608

#### 4.2 Spring Boot 自动注入及组件扫描

| 博客地址                                        | 博客说明                                                     | 备注                                        |
| ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------- |
| https://blog.51cto.com/wangguangshuo/2047667    | RequestMapping中produces属性作用                             | 设置返回数据（response data）的类型以及编码 |
| https://www.cnblogs.com/hongdada/p/9120899.html | [SpringBoot 消息转换器 HttpMessageConverter](https://www.cnblogs.com/hongdada/p/9120899.html) |                                             |
|                                                 |                                                              |                                             |

![img](https://images2018.cnblogs.com/blog/443934/201806/443934-20180601141959525-635695197.png)

### 5. http体系 

#### 5.1 springboot，output json http request body before serialize

https://stackoverflow.com/questions/33277248/spring-boot-output-json-http-requestbody-before-serialize

https://www.jianshu.com/p/333ed5ee958d

##### 5.1.1 http接口接收参数流程

```java
1、读取json http requestbody
2、deserialized it to @RequestBody object
   ObjectMapper mapper = new ObjectMapper();
   mapper.writeValueAsString(request)
```

