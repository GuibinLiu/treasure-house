## 云存储解决方案 - 阿里云 OSS

### 概述

> 对象存储服务（Object Storage Service）：缩写为 OSS
>
> 阿里云 OSS：基于网络的对象云存储服务，通过网络可以随时存储和调用包括文本、图片、音频、视频等各种非结构化数据文件



### 特点

> 将数据文件以对象（Object）的形式上传到存储空间（Bucket）



### 功能

* 可以创建一个或多个存储空间，并向每个存储空间上传一个或多个文件
* 可以修改存储空间或文件的属性或元信息来设置访问权限
* 可以获取文件的存储地址来进行分享或下载
* 可以通过阿里云管理控制台执行基本和高级的 OSS 任务
* 可以使用 Restful API 在应用程序中调用执行基本和高级的 OSS 任务



### 使用

#### 开通阿里云 OSS

1. 进入[阿里云官网](https://www.aliyun.com)，注册账号并完成实名认证

2. 充值账户

3. 选择对象存储 OSS 产品，点击进入商品页

   ![](D:\Markdown笔记\treasure-house\OSS\img\aliyun-oss.png)

4. 点击立即开通

5. 开通后可以进入管理控制台进行操作



#### Java 操作阿里云 OSS

> [文档地址](https://help.aliyun.com/product/31815.html?spm=a2c4g.750001.list.22.7e7e7b136jVkdZ)

* 创建工程并引入阿里云 OSS 依赖

  ```xml
  <dependency>
      <groupId>com.aliyun.oss</groupId>
      <artifactId>aliyun-sdk-oss</artifactId>
      <version>3.8.0</version>
  </dependency>
  ```

  

* 编写简单测试类

  ```java
  public class TestOSS {
      public static void main(String[] args) {
          
          String endpoint=""; // 服务器域名，在 Bucket 中查看
          String accessKeyId=""; // 云账号（区别于阿里云登录账号），用于调用 API
          String accessKeySecret=""; // 云账号秘钥
          
          // 创建 OSSClient 实例
          OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
          // 创建文件上传流
          InputStream is = new FileInputStream("fileUrl");
          // 上传文件
          ossClient.putObject("bucketName", "objectName", is);
          // 关闭 OSSClient
          ossClient.shutDown();
      }
  }
  ```

  
  
* Spring 整合 OSS

  ```xml
  <!-- Spring 配置文件中配置 OSSClient bean -->
  <bean id="ossClient" class="com.aliyun.oss.OSSClient">
	<property index="0" value="<endpoint>"></property>
      <property index="1" value="<accessKeyId>"></property>
    <property index="2" value="<accessKeySecret>"></property>
  </bean>
  ```
  
  ```java
  @RestController
  public class UploadController {
      
      @AutoWired
  	private OSSClient ossClient;    
      
      @PostMapping("/upload")
      public String ossUpload(@RequestParam("file") MultipartFile file,
                             HttpServletRequest request) {
          String bucketName="";
          // 保证文件名的唯一
          String fileName = UUID.randomUUID() + file.getOriginalFilename();
          
          try{
          	ossClient.putObject(bucketName, fileName, file.getInputStream());
          } catch(Exception e) {
              e.printStackTrace();
          }
          
          return "https://" + bucketName + ".<endpoint>/" + "<folder>/" +fileName;
      }
  }
  
  ```
  
  
  
  

#### 云账号获取

* 登录进入阿里云管理控制台，鼠标移动至用户头像，点击 `accesskeys` 选项

  ![](D:\Markdown笔记\treasure-house\OSS\img\accesskey1.png)

* 进入安全提示弹窗

  ![](D:\Markdown笔记\treasure-house\OSS\img\accesskey2.png)

* 点击创建 Access Key，进行手机验证

  ![](D:\Markdown笔记\treasure-house\OSS\img\accesskey3.png)

* 验证通过后即可成功创建 Access Key

  ![](D:\Markdown笔记\treasure-house\OSS\img\accesskey4.png)

  



