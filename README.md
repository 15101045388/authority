# 权限模块
###	一、权限-用户模块
1. 提供管理员和普通用户查看自己使用的接口。普通用户查看自己的接口，管理员查看所有接口。

### 二、模型
#### model
* account 用户表
  1. id 标识符
  2. name 名称
  3. appkey appkey值，验证key值
  4. secretkey 生成mac值，判断用户是否有权限访问。
  5. status 用户状态。0-正常使用，1-暂停使用，2-不能使用

* dbinfo 库详情表
  1. id 标识符
  2. woner_id 创建者
  3. name 库名
  4. createtime 创建时间
  5. status 库状态。0-正常使用，1-暂停使用，2-不能使用

* interface 接口详情表
  1. id 标识符
  2. title 接口标题
  3. description 接口描述
  4. address 访问地址
  5. method http提交方法
  6. owner_id 创建者
  7. table_id 访问的那张表
  8. create_time 创建时间
  9. type 访问类型。修改|删除|新增|查询
  10. status 0-正常使用，1-暂停使用，2-不能使用

* tableinfo 表详情表
  1. id 标识符
  2. owner_id 创建者
  3. name 表名称
  4. fieldsinfo 表字段详情
  5. createtime 创建时间
  6. description 表的描述信息
  7. status 0-正常使用，1-暂停使用，2-不能使用

* accessibities 用户接口关联表
  1. id 标识符
  2. account_id 用户id
  3. interface_id 接口id
  4. access_time 接口访问时间
  5. create_time 接口创建时间
  6. expired_at 接口到期时间
  7. create_by 谁为谁赋予这个接口权限
  8. status 0-正常使用，1-暂停使用，2-不能使用

#### controller
* accountController 处理用户请求
  1. delete 删除用户
  2. create 创建用户
  3. view（name） 根据name查询，也可以查询所有用户
  4. view（id） 根据id查询
  5. exist 判断用户是否存在
  6. resetkey 重置appkey、skey

  返回类型 restResult

* dataController 处理表信息和库信息
  1. createdb 新增一个库信息
  2. resettableinfo 重置表的状态-0、正常使用
  3. suspendtableinfo 重置表的状态-1、暂缓使用
  4. deletetableinfo 重置表的状态-2、不能使用
  5. addtableinfo 新增一个表的信息
  6. gettableinfo（id，name）可以根据id，name查询表，也可以查看所有表
  7. getdbinfo（id，name）可以根据id，name查询库，也可以查看所有库

  返回类型 restResult

* filtercontroller 权限过滤查询
  1. check（name、api、method） 判断用户当前url是否有权限访问
  2. getoneaccessibitybyid 根据id查看一个Accessibities的数据
  3. getoneinterfacebyid 根据id查看一个interface的数据
  4. addinterface 新增interface表数据
  5. getinterface 查询interface表数据
  6. addaccessibity 新增Accessibities表数据
  7. deleteinterface（id） 删除interface信息
  8. updateinterface 修改interface信息
  9. getaccessibities （account_name、interface_id）可以根据account_name、interface_id查询数据、也可以查询所有信息
  10. suspendaccessibitiestatus 修改状态-1-暂缓
  11. deleteAccessibitiesstatus 删除状态-2-不能使用

    返回类型 restResult
  
  13. searchinterfacebytable 根据表名查看interface信息
  
    返回类型 Map

### 三、接口
  用户接口
  <table>
    <tr>
      <th>序号</th>
      <th>接口名称</th>
      <th>接口地址</th>
      <th>http方法</th>
    </tr>
    <tr>
      <td>1</td>
      <td>创建用户</td>
      <td>/create</td>
      <td>POST</td>
    </tr>
    <tr>	
      <td>2</td>
      <td>根据用户名获取用户信息</td>
      <td>/users</td>
      <td>GET</td>
    </tr>
    <tr>
      <td>3</td>
      <td>根据用户id获取用户信息</td>
      <td>/users/{id}</td>
      <td>GET</td>
    </tr>
    <tr>
      <td>4</td>
      <td>判断用户是否已存在</td>
      <td>/exist</td>
      <td>GET</td>
    </tr>
    <tr>
      <td>5</td>
      <td>删除用户</td>
      <td>/delete</td>
      <td>POST</td>
    </tr>
    <tr>
      <td>6</td>
      <td>重置appkey、skey</td>
      <td>/resetkey</td>
      <td>POST</td>
    </tr>
  </table>

  权限接口
   <table>
    <tr>
      <th>序号</th>
      <th>接口名称</th>
      <th>接口地址</th>
      <th>http方法</th>
    </tr>
    <tr>
      <td>1</td>
      <td>权限验证</td>
      <td>/check</td>
      <td>GET</td>
    </tr>
    <tr>	
      <td>2</td>
      <td>新增interface表数据</td>
      <td>/interface</td>
      <td>POST</td>
    </tr>
    <tr>
      <td>3</td>
      <td>根据id查看1个interface数据</td>
      <td>/interface</td>
      <td>GET</td>
    </tr>
    <tr>
      <td>4</td>
      <td>查询interface数据</td>
      <td>/interface</td>
      <td>GET</td>
    </tr>
    <tr>
      <td>5</td>
      <td>修改interface信息</td>
      <td>/interface/{id}</td>
      <td>POST</td>
    </tr>
    <tr>
      <td>6</td>
      <td>删除interface信息</td>
      <td>/interface/{id}</td>
      <td>DELETE</td>
    </tr>
    <tr>
      <td>7</td>
      <td>新增accessibity数据</td>
      <td>/accessibities</td>
      <td>POST</td>
    </tr>
    <tr>
      <td>8</td>
      <td>根据id查看accessibities数据</td>
      <td>/accessibities/{id}</td>
      <td>GET</td>
    </tr>
    <tr>
      <td>10</td>
      <td>suspend——暂缓 状态值变为1</td>
      <td>/accessibities/{id}/suspend</td>
      <td>POST</td>
    </tr>
    <tr>
      <td>12</td>
      <td>delete——删除 状态值变为2</td>
      <td>/accessibities/{id}/delete</td>
      <td>POST</td>
    </tr>
    <tr>
      <td>13</td>
      <td>根据table查看interface信息</td>
      <td>/interface/table</td>
      <td>GET</td>
    </tr>
  </table>

### 四、测试
  * 目前可以测试14个多线程同时访问

### 五、部署环境
  1. 访问地址：http://localhost:8082
  2. 运行环境：jdk1.8、linux
  3. 数据库mysql5.6、redis2.4
