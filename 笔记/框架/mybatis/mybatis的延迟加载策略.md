## mybatis的延迟加载



1. mybatis中的延迟加载

   - 什么是延迟加载

     ​		在真正使用数据时才发起查询，不用的时候不查询。按需加载（懒加载）

     ​		例：一对多，多对多

   - 什么是立即加载

     ​		不管用不用，只要一调用方法，马上发起查询。

     ​		例：多对一，一对一

2. mybatis中的缓存

   - 什么是缓存
   - 为什么使用缓存
   - 什么样的数据可以使用缓存，什么样的数据不能使用缓存
   
3. 延迟加载的两种方式：

   1. association
   2. collection

4. mybatis的缓存

   1. 一级缓存：一级缓存是 SqlSession 级别的缓存，只要 SqlSession 没有 flush 或 close，它就存在。

      flush方法，强制内存和数据库信息同步

      ```java 
      public class UserTest { 
        private InputStream in ; 
        private SqlSessionFactory factory; 
        private SqlSession session; 
        private IUserDao userDao;
        
        @Test public void testFindById() { 
          //6.执行操作
          User user = userDao.findById(41); 
          System.out.println("第一次查询的用户："+user); 
          User user2 = userDao.findById(41); 
          System.out.println("第二次查询用户："+user2); 
          System.out.println(user == user2); 
        }
        @Before//在测试方法执行之前执行 
        public void init()throws Exception {
          //1.读取配置文件 
          in = Resources.getResourceAsStream("SqlMapConfig.xml"); 
          //2.创建构建者对象 
          SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
          //3.创建 SqlSession 工厂对象 
          factory = builder.build(in); 
          //4.创建 SqlSession 对象 
          session = factory.openSession(); 
          //5.创建 Dao 的代理对象 
          userDao = session.getMapper(IUserDao.class); }
        @After//在测试方法执行完成之后执行 
        public void destroy() throws Exception{ 
          //7.释放资源 
          session.close(); 
          in.close(); 
        }
      }
      ```

   2. 一级缓存是 SqlSession 范围的缓存，当调用 SqlSession 的修改，添加，删除，commit()，close()等方法时，就会清空一级缓存。 

   3. 二级缓存：二级缓存是 mapper 映射级别的缓存，多个 SqlSession 去操作同一个 Mapper 映射的 sql 语句，多个 SqlSession 可以共用二级缓存，二级缓存是跨 SqlSession 的。

      ​	第一步：在 SqlMapConfig.xml 文件开启二级缓存

      1. ```xml
         <settings> 
           <!-- 开启二级缓存的支持 --> 
           <setting name="cacheEnabled" value="true"/> 
         </settings>
         ```

      2. 当我们在使用二级缓存时，所缓存的类一定要实现 java.io.Serializable 接口，这种就可以使用序列化 方式来保存对象。 