# 使用Python读文件

Dataphin仅支持开发基于Python的脚本，不支持开发依赖第三方组件的脚本。开发基于第三方组件的脚本，需要通过pip install下载第三方组件。本文为您介绍基于Dataphin如何通过构建Shell任务调用Python读取第三方文件。

-   添加访问地址mirrors.aliyun.com和端口\*至项目空间的沙箱白名单，详情请参见[设置白名单](/cn.zh-CN/RDS MySQL 数据库/快速入门/设置白名单.md)。
-   已准备Python支持读取的文件，例如TXT、CSV、XLS、XLSX或PDF等格式文件。****

## 步骤一：上传文件

1.  登录[Dataphin控制台](https://dataphin.console.aliyun.com/workingArea)。

2.  在Dataphin控制台页面，选择工作区地域后，单击**进入Dataphin\>\>**。

3.  进入**资源管理**页面。

    1.  在Dataphin首页，单击**研发**。

    2.  在数据**开发**页面，单击**数据处理**。

    3.  在左侧导航栏，单击![资源管理](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2987549951/p96911.png)**资源管理**图标。

4.  在**资源管理**页面，单击**资源管理**后的![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8397549951/p72394.png)图标。

5.  在**新建资源**对话框中，配置参数。

    ![rea](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1676057061/p185184.png)

    |参数|描述|
    |--|--|
    |**类型**|选择**others**。|
    |**名称**|上传文件的名称需要以文件类型结尾。例如test.xlsx。|
    |**描述**|填写资源的描述。|
    |**上传文件**|选择本地的文件，例如test.xlsx。|
    |**计算类型**|选择**无归属引擎**。**说明：** 文件资源存储至Dataphin系统，因此仅支持选择**无归属引擎**。 |
    |**选择目录**|默认为**资源管理**。|

6.  单击**提交**，完成资源的提交。

7.  在**提交备注**对话框，填写备注信息。

8.  单击**确定并提交**。


## 步骤二：创建Shell任务

1.  在**数据处理**页签，单击左侧导航栏![agaga](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8980863061/p176215.png)**计算任务**图标。

2.  在**计算任务**页面，单击**计算任务**后的![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8397549951/p72394.png)图标，选择**通用脚本** \> **SHELL**。

3.  编写DataX任务代码。

    1.  在**新建文件**对话框，配置参数。

        ![test](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3199826061/p185143.png)

        |参数|描述|
        |--|--|
        |**名称**|填写计算任务的名称，例如Python读取文件。|
        |**调度类型**|选择任务的调度类型为**周期性节点**。|
        |**描述**|填写对任务的简单描述。|
        |**选择目录**|系统自动选择为**代码管理**。|

    2.  单击**确定**。


## 步骤三：编写并运行Shell任务代码

1.  在代码编写页面，编写代码。

    ```
    #在Dataphin的Linux服务器上新建目录。
    mkdir -p /tmp/chars/ && \
           
    #指定目录/tmp/chars/为python源。
    pip install -i https://mirrors.aliyun.com/pypi/simple/ \
    --target=/tmp/chars/ \
    openpyxl
    
    #指定的python源写入至openfile.py。
    cat >openfile.py <<EOF
    
    @resource_reference{"test.xlsx"}
    # -*- coding:utf-*-
    import os
    import sys
    sys.path.append('/tmp/chars/')
    import openpyxl
    print '========= python execute ok =========='
    print("start===============")
    args = sys.argv
    # 打开excel文件，获取sheet名
    wb = openpyxl.load_workbook(args[1])
    #  wb.get_sheet_names 这个方法已过时 会有一个警告
    print(wb.worksheets[0])
    
    EOF
    #python中调用文件。
    python openfile.py test.xlsx
    ```

    其中，test.xlsx参数需要替换为您已上传的文件。

2.  单击页面右上角的**执行**，即可运行任务代码。

    运行结果的状态为SUCCESS，表示读取文件成功。

    ![test](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6629036061/p185182.png)


