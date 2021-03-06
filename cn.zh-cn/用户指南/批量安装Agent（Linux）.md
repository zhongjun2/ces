# 批量安装Agent（Linux）<a name="ZH-CN_TOPIC_0127544062"></a>

## 操作场景<a name="zh-cn_topic_0111157443_section476316142516"></a>

本章节指导用户在已有的大量弹性云服务器（Linux）上批量安装Agent。

## 操作流程<a name="section149797195411"></a>

选择其中一台ECS绑定弹性IP后，参照[安装配置Agent（Linux）](安装配置Agent（Linux）.md)安装Agent并配置，确保数据采集正常。将此虚拟机作为跳板机通过批量执行脚本依次将Agent包和配置文件拷贝、解压、执行安装到其他虚拟机中。

>![](public_sys-resources/icon-notice.gif) **注意：**   
>-   批量安装的ECS需同属一个VPC下。  
>-   若第一台ECS手动配置了AK/SK，则之后的ECS无需再配置；若第一台ECS是通过在主机监控界面通过插件配置创建委托的，则之后的ECS也需在主机监控界面通过插件配置创建委托。  
>-   Windows版本暂不支持批量安装Agent。  

## 前提条件<a name="zh-cn_topic_0111157443_section57291745182517"></a>

-   已收集需要安装Agent的所有虚拟机IP，按照iplist.txt格式整理好，并上传到第一台机器的/usr/local目录下

    >![](public_sys-resources/icon-note.gif) **说明：**   
    >iplist.txt格式示例如下所示（多台ECS密码一致，可只填写IP）。  
    >```  
    >192.168.1.1  
    >192.168.1.2  
    >```  
    >多台ECS密码不同，建议IP、ID、密码都填写，ID与密码之间用空格隔开。  
    >样例中abcd为密码，请按实际值填写。  
    >```  
    >192.168.1.1:bdf784cb-xxxx-xxxx-xxxx-bf0a99d8d486 abcd  
    >192.168.1.2:cgvfa4e4-xxxx-xxxx-xxxx-b32231553926 abcd  
    >```  


## 操作步骤<a name="zh-cn_topic_0111157443_section335374517325"></a>

1.  使用Putty以root用户登录到第一台弹性云服务器中。
2.  执行如下命令，下载batchInstallAgent.tar.gz。

    华北-北京一：

    **cd /usr/local && wget http://telescope.obs.cn-north-1.myhwclouds.com/scripts/agentBatchPackage.sh && chmod 755 agentBatchPackage.sh && ./agentBatchPackage.sh**

    华南-广州：

    **cd /usr/local && wget http://telescope-cn-south-1.obs.cn-south-1.myhwclouds.com/scripts/agentBatchPackage.sh && chmod 755 agentBatchPackage.sh && ./agentBatchPackage.sh**

    华东-上海二：

    **cd /usr/local && wget http://telescope-cn-east-2.obs.cn-east-2.myhwclouds.com/scripts/agentBatchPackage.sh && chmod 755 agentBatchPackage.sh && ./agentBatchPackage.sh**

    亚太-香港：

    **cd /usr/local && wget https://telescope-ap-southeast-1.obs.ap-southeast-1.myhwclouds.com/scripts/agentBatchPackage.sh && chmod 755 agentBatchPackage.sh && ./agentBatchPackage.sh**

    亚太-曼谷：

    **cd /usr/local && wget https://telescope-ap-southeast-2.obs.ap-southeast-2.myhwclouds.com/scripts/agentBatchPackage.sh && chmod 755 agentBatchPackage.sh && ./agentBatchPackage.sh**

3.  执行如下命令，运行脚本并输入密码（多台ECS密码一致）。

    **cd /usr/local && ./batchInstall.sh $password**

    >![](public_sys-resources/icon-notice.gif) **注意：**   
    >-   如果配置的iplist.txt中涉及多个密码则多次输入上述命令和密码，密码不对的ECS则会安装失败。  
    >-   多台ECS密码不同时，请执行**cd /usr/local && ./batchInstall.sh。**  
    >-   脚本执行过程中需保证虚拟机正常开机状态。  

4.  安装完成后，登录云监控管理控制台，单击左侧导航栏的“主机监控”，即可查看所有已安装Agent的弹性云服务器列表。

    >![](public_sys-resources/icon-note.gif) **说明：**   
    >Agent插件配置完成后，因监控数据暂未上报，插件状态仍显示“未安装”，等待3-5分钟，刷新即可。  

5.  （可选）安装完成后如果不需要pexpect模块，则执行如下命令，到python安装目录下删除pexepct 和ptyprocess模块。

    **cd /usr/lib/python2.7/site-packages**

    **rm pexpect-3.2-py2.7.egg-info -f**

    **rm ptyprocess-0.5.2-py2.7.egg-info -f**

    **rm pexpect -rf**

    **rm ptyprocess -rf**


