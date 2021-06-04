## 软件考试初级错题整理

### 1.office相关

![office1](collection_wrong.assets\office1-1622104701904.png)

![office2](collection_wrong.assets\office2.png)

![多媒体1](collection_wrong.assets\多媒体1.png)

### 2.程序设计

- 编译器：
  - 词法分析：输入源程序，输出记号流。（关键字、标识符、字面量、特殊符号）
  - 语法分析：构造语法树
  - 语义分析：类型检查，转换，保证语义合法
  - 中间代码生成：与具体机器无关
  - 中间代码优化：中间代码在空间上很大浪费，需要优化
  - 目标代码生成：汇编语言、可重定位二进制代码、内存形式
  - 符号表管理：记录源程序中符号的必要信息
  - 出错处理：动态错误、静态错误
- UML图
  - 序列图：描述时间顺序组织的对象之间交互、
  - 通信图：收发消息
  - 活动图：系统内从一个活动到另一个活动，业务过程
  - 交互概览图：关注控制流
  - 类图：包含类、接口、协作和它们之间的依赖，常用于对系统的词汇进行建模
  - 组件图：代码构件的物理结构以及各种构建之间的关系
  - 包图：模型本身组织层次结构
  - 部署图：静态实施视图

![程序设计1](collection_wrong.assets\程序设计1.png)

![程序设计11](collection_wrong.assets\程序设计11.png)

![程序设计2](collection_wrong.assets\程序设计2.png)

![程序设计4](collection_wrong.assets\程序设计4.png)

![程序设计10](collection_wrong.assets\程序设计10.png)

![程序设计5](collection_wrong.assets\程序设计5.png)

![程序设计6](collection_wrong.assets\程序设计6-1622106701984.png)

![程序设计7](collection_wrong.assets\程序设计7.png)

![程序设计8](collection_wrong.assets\程序设计8.png)

![程序设计9](collection_wrong.assets\程序设计9.png)



### 3.软件工程

![软件工程0](collection_wrong.assets\软件工程0.png)

![软件工程1](collection_wrong.assets\软件工程1.png)

![软件工程2](collection_wrong.assets\软件工程2.png)

![软件工程3](collection_wrong.assets\软件工程3.png)

![软件工程4](collection_wrong.assets\软件工程4.png)

![软件工程5](collection_wrong.assets\软件工程5.png)

![image-20210528144520152](collection_wrong.assets\image-20210528144520152.png)

### 4.数据结构&算法

![数据结构1](collection_wrong.assets\数据结构1.png)

![数据结构2](collection_wrong.assets\数据结构2.png)

![数据结构3](collection_wrong.assets\数据结构3.png)

![数据结构4](collection_wrong.assets\数据结构4.png)

![数据结构5](collection_wrong.assets\数据结构5.png)

![数据结构7](collection_wrong.assets\数据结构7.png)

### 5.网络

- 电子邮件协议：SMTP、POP3、MAP4；端口25、110/143
- 电子邮件发送多媒体文件：MIME
- windows操作系统，FTP组件集成在IIS中

![网络1](collection_wrong.assets\网络1.png)

![网络3](collection_wrong.assets\网络3.png)

![网络4](collection_wrong.assets\网络4.png)



### 6.系统

- 微机系统总线
  - 带宽：单位时间内总线上传的数据量
  - 位宽：能同时传送二进制数据的位数
  - 工作频率：工作时钟频率MHz为单位
- 流水线技术：pipeline 是指在程序执行时多条指令重叠进行操作的一种准并行处理实现技术
- I/O接口与主机交换数据的方式
  - 程序查询方式：CPU执行程序查询外设状态
  - 中断方式：程序控制I/O，CPU必须等待I/O完成数据传输
  - 并行控制方式：
  - DMA：CPU交出计算机的控制权，不参与内存与外设的数据交换，内存与外设数据直接传送
  - 无条件传送：外设无条件接受CPU的输出数据，无条件向CPU提供需要输入的数据
- Cache的作用是解决CPU与主存间的速度匹配问题
- 计算机系统的可靠性通常用**平均故障间隔时间**来衡量
- 程序计数器是用于存放下一条指令所在单元的地址
- 控制单元 获取指令并进行分析
- 寻址方式
  - 直接寻址：操作数在内存中，指令给出操作数的地址，访问内存来获得操作数
  - 立即寻址：操作数在指令中
  - 间接寻址：指令给出操作数地址的地址
  - 寄存器寻址：操作数在寄存器中
  - 寄存器间接寻址：操作数的地址在CPU寄存器中，还要访问一次内存来得到操作数
  - 相对寻址：指令地址给出一个偏移量
  - 变址寻址：
- 编译是将高级语言源代码转换成目标代码的过程，运行速度快
- 解释不生成目标代码，一条条解释，运行速度慢

![系统1](collection_wrong.assets\系统1.png)

![系统2](collection_wrong.assets\系统2.png)

![系统3](collection_wrong.assets\系统3.png)

![系统5](collection_wrong.assets\系统5.png)

![系统10](collection_wrong.assets\系统10.png)

![系统11](collection_wrong.assets\系统11.png)

![系统12](collection_wrong.assets\系统12.png)

![系统15](collection_wrong.assets\系统15.png)

![系统16](collection_wrong.assets\系统16.png)

![系统17](collection_wrong.assets\系统17.png)

![系统18](collection_wrong.assets\系统18.png)

![系统19](collection_wrong.assets\系统19.png)

![系统21](collection_wrong.assets\系统21.png)

![系统22](collection_wrong.assets\系统22.png)

![系统24](collection_wrong.assets\系统24.png)



### x.其他

- 数据模型的三要素：数据结构、数据操作、完整性约束
- E-R模型：实体、联系、属性
- 死锁原因
  - 互斥条件：一次只允许一个进程使用
  - 请求保持条件：阻塞
  - 不可剥夺条件：只能自己释放
  - 环路条件：互为条件

![image-20210528144827244](collection_wrong.assets\image-20210528144827244.png)

![数据库](collection_wrong.assets\数据库.png)

![信技1](collection_wrong.assets\专英1.png)

![系统9](collection_wrong.assets\系统9.png)

![image-20210528144612680](collection_wrong.assets\image-20210528144612680.png)

![image-20210528144707997](collection_wrong.assets\image-20210528144707997.png)



![image-20210528171219350](collection_wrong.assets\image-20210528171219350.png)