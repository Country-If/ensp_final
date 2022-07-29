[TOC]

# 腾科考核

## 写在前面

1. markdown统一格式，图片放在README.assets下

   ![image-20220726170040090](README.assets/image-20220726170040090.png)

2. 由于配置的保存不一定ok(被坑很多次了)，因此把主要设备的配置文件备份后放在`config_backup`文件夹下以防不测

2. 拓扑图有更新时记得把图片更新了，文件名什么的随便搞就行，typora能看就行

3. 步骤中，每个大的步骤用**三级标题**，具体实现的小步骤加**数字标号**，同级操作(同类设备)用**普通标号**；每一步配置截图中需要框画出重要的操作(如命令、具体操作步骤等)

4. 各设备的保存：在用户模式下(输入`sy`进入全局模式，`ctrl + z`可以退出全局模式进入用户模式)，键入命令`save all`后回车，键入`y`保存配置，Git提交时连同设备的配置都要提交

## 拓扑图

![image-20220729002039616](README.assets/image-20220729002039616.png)

## 网络设计

### 地址划分

- PC1 处在vlan 10，网段：192.168.10.0/24

- PC2 处在vlan 10，网段：192.168.20.0/24

- PC3 网段：192.168.30.0/24

- PC4、PC5 网段：192.168.40.0/24

- PC6 网段：110.1.1.0/24

- PC10 网段：120.1.1.0/24

- Server1、Server2、Server3 网段：192.168.100.0/24

### 安全区域划分

- FW1

  trust区域：`g1/0/0`，`g1/0/0.10`，`g1/0/0.20`

  untrust区域：`g1/0/1`，`g1/0/2`，`g1/0/3`

- FW2

  trust区域：`g1/0/2`

  untrust区域：`g1/0/3`

- FW4

  trust区域：`g1/0/0`

  untrust区域：`g1/0/3`

- FW5

  trust区域：`g1/0/1`

  untrust区域：`g1/0/2`，`g1/0/0`
  
- FW7

  trust区域：`g1/0/1`

  untrust区域：`g1/0/0`

### 配置单臂路由让vlan 10和vlan 20内部可以通信

### 配置NAT策略：easy-ip、公网地址转换

### 配置IPSec VPN

### 配置双机热备

## 步骤

### 各设备地址设置

- PC1

  ![image-20220727102602211](README.assets/image-20220727102602211.png)

- PC2

  ![image-20220727102825278](README.assets/image-20220727102825278.png)

- PC3

  ![image-20220727102909990](README.assets/image-20220727102909990.png)

- PC4

  ![image-20220727103006843](README.assets/image-20220727103006843.png)

- PC5

  ![image-20220727150149499](README.assets/image-20220727150149499.png)

- PC6

  ![image-20220728221449466](README.assets/image-20220728221449466.png)

- PC10

  ![image-20220729084516871](README.assets/image-20220729084516871.png)

- Server1

  ![image-20220727103146189](README.assets/image-20220727103146189.png)

- Server2

  ![image-20220727172627332](README.assets/image-20220727172627332.png)

- Server3

  ![image-20220727103311520](README.assets/image-20220727103311520.png)

### 防火墙配置网页端访问

- FW1

  ![image-20220726172131707](README.assets/image-20220726172131707.png)

  补充：g0/0/0端口ip地址不变，仍为192.168.0.1/24

- FW2

  ![image-20220726172304913](README.assets/image-20220726172304913.png)

  补充：g0/0/0端口ip地址改为192.168.0.2/24

- FW3

  ![image-20220726172421683](README.assets/image-20220726172421683.png)

  补充：g0/0/0端口ip地址改为192.168.0.3/24

- FW4

  ![image-20220726172536797](README.assets/image-20220726172536797.png)

  补充：g0/0/0端口ip地址改为192.168.0.4/24

- FW5

  ![image-20220726172640822](README.assets/image-20220726172640822.png)
  
  补充：g0/0/0端口ip地址改为192.168.0.5/24

### 配置单臂路由

1. 交换机 LSW3 配置

   ![image-20220727112240931](README.assets/image-20220727112240931.png)

   ![image-20220727112327674](README.assets/image-20220727112327674.png)

2. 防火墙 FW1 配置

   ![image-20220727112634852](README.assets/image-20220727112634852.png)

   ![image-20220727112811666](README.assets/image-20220727112811666.png)

### 配置IPSec VPN

1. 基础配置、接口地址以及安全区域

   - FW2

     ![image-20220727122940169](README.assets/image-20220727122940169.png)

   - FW4

     ![image-20220727123103036](README.assets/image-20220727123103036.png)

2. 配置到对端的路由

   - FW2

     ![image-20220727133421795](README.assets/image-20220727133421795.png)

   - FW4

     ![image-20220727133543896](README.assets/image-20220727133543896.png)

3. 配置相关的安全策略，允许网络A与网络B互访，允许IKE协商报文及加密报文通过

   - FW2

     ![image-20220727151056104](README.assets/image-20220727151056104.png)

   - FW4

     ![image-20220727151417015](README.assets/image-20220727151417015.png)

4. 配置IPSec策略

   - FW2

     ![image-20220727152200849](README.assets/image-20220727152200849.png)

   - FW4

     ![image-20220727152300693](README.assets/image-20220727152300693.png)

5. 防火墙配置安全策略

   - FW2

     ![image-20220727153605072](README.assets/image-20220727153605072.png)

   - FW4

     ![image-20220727153626568](README.assets/image-20220727153626568.png)

### 在配置IPSec VPN的基础上添加优先规则拒绝PC5与 外部通信

1. 添加拒绝PC5出站的安全策略，并将其移到顶部

   ![image-20220727155427690](README.assets/image-20220727155427690.png)

2. 查看安全策略，可以看到上面定义的规则确实已在顶部

   ![image-20220727155531892](README.assets/image-20220727155531892.png)

3. 同理添加拒绝访问PC5数据包的入站策略，并移到顶部

   ![image-20220729080321712](README.assets/image-20220729080321712.png)

### 防火墙使用NAT技术

1. Server组使用公网地址(12.34.56.0/24)代表内部服务器对外地址

   1. 划分安全区域以及接口地址配置

      ![image-20220727205452481](README.assets/image-20220727205452481.png)

      ![image-20220727205823824](README.assets/image-20220727205823824.png)

   2. 添加安全策略

      ![image-20220727173732287](README.assets/image-20220727173732287.png)

   3. 添加nat策略

      ![image-20220727205714303](README.assets/image-20220727205714303.png)

2. 防火墙FW1配置easy-ip让内部pc可以访问外网

   - FW1配置

     ![image-20220727210237854](README.assets/image-20220727210237854.png)

### 配置OSPF

- AR1

  ![image-20220728194513266](README.assets/image-20220728194513266.png)

- AR2

  ![image-20220728194550285](README.assets/image-20220728194550285.png)

- AR3

  ![image-20220728194641998](README.assets/image-20220728194641998.png)

  补充：修改`g0/0/2`端口ip地址为110.1.1.254/24

- FW1

  ![image-20220728193356285](README.assets/image-20220728193356285.png)
  
  ![image-20220728193817638](README.assets/image-20220728193817638.png)

### 服务器组在NAT基础上添加双机热备

1. 基础配置（ip和安全区域dmz）

​		FW5接口g1/0/1 ip：10.2.0.2/24

​		FW5接口g1/0/0 ip：10.1.0.2/24

![](README.assets/image-20220729014401013.png)

​		建立安全区域dmz 

![image-20220729021856689](README.assets/image-20220729021856689.png)

![image-20220729022310094](README.assets/image-20220729022310094.png)

2. vrrp协议和心跳接口

   FW5：两边接口的ip虚拟成左右网段的网关

   ![image-20220729020049206](README.assets/image-20220729020049206.png)

   FW7同理：两边接口的ip虚拟成左右网段的网关。激活改成备份。

   ![image-20220729020439337](README.assets/image-20220729020439337.png)

   心跳接口：

   ![image-20220729022147203](README.assets/image-20220729022147203.png)

   ![image-20220729022350949](README.assets/image-20220729022350949.png)

3. 允许备份防火墙进行配置

   ![image-20220729022958001](README.assets/image-20220729022958001.png)	

4. 调通PC10 ping 服务器

   FW7防火墙安全区域划分和安全策略（备份防火墙）

   ![image-20220729023521613](README.assets/image-20220729023521613.png)

   ![image-20220729024113528](README.assets/image-20220729024113528.png)
   
   与FW5配置相同的NAT策略
   
   ![image-20220729025607741](README.assets/image-20220729025607741.png)

## 结果

### 不同vlan内主机可以互相ping通

- PC1 ping PC2

  ![image-20220727111934212](README.assets/image-20220727111934212.png)

- PC2 ping PC1

  ![image-20220727112038779](README.assets/image-20220727112038779.png)

### OSPF

- PC1和PC2 ping PC6

  ![image-20220728221707926](README.assets/image-20220728221707926.png)

  ![image-20220728221732946](README.assets/image-20220728221732946.png)

### IPSec配置后的主机通信

- PC3 ping PC4、PC5

  ![image-20220727153923601](README.assets/image-20220727153923601.png)

- PC4 ping PC3

  ![image-20220727154154779](README.assets/image-20220727154154779.png)

### 拒绝PC5对外通信

- PC5 ping 本地环境下的PC4以及外部网络中的PC3

  ![image-20220727155733990](README.assets/image-20220727155733990.png)
  
- PC4 ping 本地环境下的PC5以及外部网络中的PC3

  ![image-20220729080616515](README.assets/image-20220729080616515.png)

- 外部的PC3 ping 本地的PC4和PC5

  ![image-20220729080730251](README.assets/image-20220729080730251.png)

### 双机热备

- PC10 ping Server1_ip：12.34.56.1（公网ip）

  ![image-20220729024514107](README.assets/image-20220729024514107.png)

- 关闭主防火墙接口

  ![image-20220729030337841](README.assets/image-20220729030337841.png)

## 答辩演示

- PC1和PC2可以互相ping通
- PC4和PC5可以相互ping通，PC3和PC4可以相互ping通，PC3和PC5不可以ping通
- PC1可以ping通PC6，抓包可以看到数据包来自101.1.1.0网段，关闭AR1的接口，通过抓包可以看到数据包更改来自103.1.1.0网段
- PC10可以ping通服务器组，查看session表可以看到NAT的转换
- PC10ping通server1，防火墙FW5关闭接口后出现短暂丢包后恢复通信

## 总结

### 徐国涛

通过这次考核，复习并且实践过去一周多时间内所学的知识，有了更深的体会。在应用不同的策略配置下将网络调通，除了需要理解策略配置原理及过程，还需要细心，因为自己的各种粗心而浪费了太多的时间。在配置过程中最重要的是通过各种查看命令去查看配置进行分析，以及通过抓包分析数据包的走向。最后，由于自己的粗心问题以及不可抗力因素(电脑死机设备配置没有备份上)浪费了过多的时间，这次的考核完成的并没有想象中的好，只能说尽力了QAQ。

### 张伟龙

在家没有空调几近融化于35°的高温无法思考，也只有跟着国涛同学才能顺利完成这个实验，整个课程学到了很多，特别是跟着国涛同学不会的都会了因为他基本每个实验都完成了，我俩还进行了几次腾讯会议，我的自发不耐心在他的特别耐心下反而收敛了起来，心情复杂，望以后不拖大佬后腿，再接再厉，后面一起加油！感谢！

## GitHub

[仓库地址(暂时为](https://github.com/Country-If/ensp_final.git)

