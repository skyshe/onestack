Just checkout and run it!

1. Setup a fresh Ubuntu Precise(12.04) OS.

2. checkout
```
svn checkout http://onestack.googlecode.com/svn/trunk/ onestack-read-only
```
3. run it!
```
cd onestack-read-only/ && ./oneStack.sh
```


OR
**simply（说明：只需要设置脚本开头处多行#之间的内容，即直至/etc/network/interfaces）**
```
wget http://onestack.googlecode.com/files/oneStack.sh && chmod +x oneStack.sh && ./oneStack.sh
```


**喜欢git或者github的请去 https://github.com/Kayven/OneStack**


---

不希望同行们把过多精力花在OpenStack的安装部署上（目前OpenStack没有提供简单的安装，以后应该会改进），而是对其机制原理、工程实践、应用服务等深入研究探讨。
所以提供这个一键部署的工具，帮助大家快速建立环境生产实践。OneStack的来源、诸多考虑、安装部署、未来预期等在vpsee上有说明，可以参见[OneStack：Ubuntu 上一键自动部署 OpenStack](http://www.vpsee.com/2012/07/onestack-all-in-one-installation-tool-for-openstack/)。
安装问题可以参考《[OpenStack安装部署管理中常见问题解决方法（OpenStack-Lite-FAQ）](http://blog.csdn.net/hilyoo/article/details/7746634)》。

---


如果你是单机可以参考以下**简单安装步骤**:

1、切换到root，oneStack.sh删除设置root密码、设置locale、设置apt这3段(为了方便刚安装新系统的用户加入的可选的步骤，setup\_base.sh没有这3段)

2、设置ip等参数
OUT\_IP 外网ip（注意这是相对于OpenStack内部网络的）
OUT\_IP\_PRE 外网ip前缀（脚本后面出现ip不用管，会被这个参数替换）
FLOAT\_IP 浮动ip

3、裸机的话qemu改成kvm （对于xen之类的未加入支持）
VIRT\_YPE

4、网络设置，会替换掉你原来的，不想替换就删掉这一段
设置cat写入interfaces文件（单网卡去掉eth1的设置即可）

5、执行oneStack.sh 或者setup\_base.sh（基本系统，没有添加镜像和实例，可以setup\_test.sh）

其它没有需要更改的（数据库密码等自行更改无影响）。
里面有个image是从ubuntu官网下载，可能需要一些时间
（svn checkout，里面还有一些删除之类的工具）



---

  * 在Ubuntu（12.04/11.10）上一键安装部署Opentack Essex
    1. 只需要一个文件即可完成全部部署，自动安装，设置好参数后不需要交互输入（包括mysql）：http://onestack.googlecode.com/files/oneStack.sh
    1. 这是一个完整的部署控制节点的工具，计算节点只需要安装ntp、nova-compute，执行addComputeNode.sh即可（修改脚本里的ip配置），可以自己随便添加和更改。
    1. 遵循OpenStack的部署步骤，里面含有详细的注释说明，看完整个脚本相当于看完了安装文档和依赖关系；
    1. svn只是多一些辅助工具，包括重置、重新安装、卸载、添加nova计算节点、添加客户端节点（这是命令行管理OpenStack的节点，不是必需的）等。
    1. 也欢迎同道人补充和完善更多的功能，适用于更多的操作系统和应用场景。


---

  * **需要注意的地方**

  1. root权限执行：里面没有使用sudo因此需要root权限；脚本开头会检查并设置root密码并切换到root，可以自己注释掉。
  1. 为了方便，参数配置直接在脚本开头30行起设置，包括数据库账号密码、网络设置（双网卡）、虚拟技术kvm还是qemu，Token/dashboard登录密码。
  1. 除了开头切换root需要输入密码（可注释掉），后面的安装数据库和phpmyadmin等均不需要等待、不需要输入，可以放心让其自动安装。
  1. 系统会安装Ubuntu12.04的镜像，并启动一个实例。这个过程中镜像自动从ubuntu官网下载，可以查找cloud-images更换地址或者镜像precise-server-cloudimg-amd64-disk1.img  。也可以注释掉这个步骤，直接使用dashboard在web添加镜像启动实例。
  1. setup\_base.sh/setup\_test.sh分两步部署，以上过程就免去了。


---


这是一个一键部署OpenStack的工具。目前能够完整而正确在Ubuntu12.04（precise）安装部署OpenStack，其它Linux系统没有做，欢迎补充和完善。
  * 一键完整部署OpenStack，可以自定义配置，无需交互；
  * 安装过程不需要等待提示和输入配置：
    1. mysql密码可以自行配置，也可以使用默认的，不需要等待mysql等程序安装的提示；
    1. 数据库密码可以自行配置，全部完整安装和部署；
    1. 网络配置可以自行定义；
    1. 配置文件和依赖关系已经处理；
    1. 设置变量配置kvm或者虚拟机配置qemu
  * 默认安装一个Ubuntu12.04的操作系统镜像，并启动一个实例：
    1. 默认启动一个实例，通过运行状态可以查看是否正确部署和运行；
    1. 通过dashboard进行web管理和查看，或者nova命令管理。
  * 经过多次测试，完整在VMware虚拟机上部署OpenStack，自己可以添加swift对象存储（暂时没有加入脚本，很方便加入）。

  * 运行过程会做如下工作：
    1. 配置网络相关；
    1. 安装和配置数据库；
    1. 安装和部署身份管理keystone；
    1. 安装和部署镜像管理glance；
    1. 安装和部署控制计算nova；
    1. 安装和部署web前端dashboard；
    1. 上传和添加ubuntu12.04镜像；
    1. 设置项目安全规则；
    1. 启动实例，并正常运行。

  * 功能齐全，附带了卸载、重置、添加计算节点等工具
    1. 可以卸载安装的opentack组件，包括nova、glance、keystone等；
    1. 可以重置数据库和配置，重新安装openstack组件；
    1. 可以根据需要自行更改脚本，方便部署自己的云计算平台。


---

**项目结构**

  * 1、一键部署All-in-one的OneStack实验环境
> > 只需要一个文件：oneStack.sh
  * 2、一键部署OneStack控制节点，任意添加计算节点
> > 控制节点：oneStack.sh （可以删掉nova-compute）
> > 计算节点：addComputeNode.sh

  * 3、OneStack的卸载、重置和清空等
> > 使用root权限执行
    1. . delete OpenStack
> > > ./delStack.sh # 只卸载nova、glance、keystone等

> > 2). delete all
> > > ./delAll.sh #卸载所有安装的组件和工具

> > 3). reset OpenStack
> > > ./resetStack.sh clear # 清空数据库，镜像、网络和实例等
> > > ./resetStack.sh

  * 4、OneStack添加客户端、分步安装

> > 添加客户端，nova管理等 ./addClient.sh
> > 安装基本系统 ./setup\_base.sh
> > 添加镜像和实例，设置见脚本里面的说明 ./setup\_test.sh

  * 5、OneStack的高可用性
> > 这是需要添加的部分，在目录HAStack下，希望更多人可以提出自己的解决方案。
> > 详见文章 csdn[《构建OpenStack的高可用性（HA，High Availability） 》](http://blog.csdn.net/hilyoo/article/details/7704280)对高可用性OpenStack的讨论。

---


  * 欢迎反馈（ Hily.Hoo@gmail.com ），谢谢.
    1. 本人尽力做到不出差错，但是限于学识和眼界，难免有考虑不周、冗余、没有最优化或者表达不好之处；
    1. 希望大家有任何意见建议随时联系我。