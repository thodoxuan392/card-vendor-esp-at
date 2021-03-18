Web Server AT 示例
==================

本文档主要介绍 AT web server 的使用，主要涉及以下几个方面的应用：

.. contents::
   :local:
   :depth: 1

使用浏览器进行 Wi-Fi 配网
--------------------------

简介
^^^^

浏览器配网是通过浏览器连接 ESP 设备创建的 AP，并通过 HTTP 请求将需要连接的路由器信息传输给 ESP 设备，ESP 设备通过这些信息连接到对应的路由器，并通知浏览器配网结果的解决方案。

流程
^^^^

整个配网流程可以分为以下三步：  

.. contents::
   :local:
   :depth: 1

配网设备连接 ESP 设备
""""""""""""""""""""""""

首先，为了让配网设备连接 ESP 设备，ESP 设备需要配置成 AP + STA 模式， 并创建一个 WEB 服务器等待配网信息，对应的 AT 命令如下：

#. 清除之前的配网信息，如果不清除配网信息，可能因为依然保留之前的配置信息从而导致 WEB 服务器无法创建。


   - Command
   
     ::
 
       AT+RESTORE

#. 配置 ESP 设备为 Station + SoftAP 模式。


   - Command
   
     ::
 
       AT+CWMODE=3

#. 设置 SoftAP 的 ssid 和 password（如设置默认连接 ssid 为 `pos_softap`，无密码的 Wi-Fi）。


   - Command
   
     ::
 
       AT+CWSAP="pos_softap","",11,0,3

#. 使能多连接。


   - Command
   
     ::
 
       AT+CIPMUX=1

#. 创建 WEB server，端口：80，最大连接时间：25 s（默认最大为 60 s）。


   - Command
   
     ::
 
       AT+WEBSERVER=1,80,25

然后，使用上述命令启动 web server 后，打开配网设备的 Wi-Fi 连接功能，连接 ESP 设备的 AP：

.. figure:: ../../_static/Web_server/web_brower_wifi_ap.png
   :align: center
   :alt: 浏览器连接 ESP AP
   :figclass: align-center

   浏览器连接 ESP AP

使用浏览器发送配网信息
""""""""""""""""""""""""

在配网设备连接到 ESP 设备后，即可发送 HTTP 请求，配置待接入的路由器的信息：（注意，浏览器配网不支持配网设备作为待接入 AP）。
在浏览器中输入 WEB server 默认的 IP 地址（如果未设置 ESP 设备的 SoftAP IP 地址，默认为 192.168.4.1，您可以通过 AT+CIPAP? 命令查询当前的 SoftAP IP 地址)，打开配网界面，输入拟连接的路由器的 ssid、password，点击“开始配网”即可开始配网：

.. figure:: ../../_static/Web_server/web_brower_open_html.png
   :align: center
   :alt: 浏览器打开配网界面
   :figclass: align-center

   浏览器打开配网界面

用户也可以点击配网页面中 SSID 输入框右方的下拉框，查看 ESP 模块附近的 AP列表，选择目标 AP 并输入 password 后，点击“开始配网”即可启动配网：

.. figure:: ../../_static/Web_server/web_brower_get_ap_record.png
   :align: center
   :alt: 浏览器获取 Wi-Fi AP 列表
   :figclass: align-center

   浏览器获取 Wi-Fi AP 列表示意图

通知配网结果
""""""""""""""""

配网成功后网页显示如下：

.. figure:: ../../_static/Web_server/web_brower_wifi_connect_success.png
   :align: center
   :alt: 浏览器配网成功
   :figclass: align-center

   浏览器配网成功

**说明** 1：配网成功后，网页将自动关闭，若想继续访问网页，请重新输入 ESP 设备的 IP 地址，重新打开网页。

同时，在串口端将收到如下信息：

::

    +WEBSERVERRSP:1      // 代表启动配网  
    WIFI CONNECTED       // 代表正在连接  
    WIFI GOT IP          // 连接成功并获取到 IP  
    +WEBSERVERRSP:2      // 代表网页端收到配网成功结果，此时可以释放 WEB 资源  

如果配网失败，网页将显示：

.. figure:: ../../_static/Web_server/web_brower_wifi_connect_fail.png
   :align: center
   :alt: 浏览器配网失败
   :figclass: align-center

   浏览器配网失败

同时，在串口端将收到如下信息：

::

    +WEBSERVERRSP:1      // 代表启动配网，没有后续发起连接以及获取 IP 的信息，MCU 可以在收到该条消息后建立计时，若计时超时，则配网失败。

常见故障排除
^^^^^^^^^^^^

**说明** 1：配网页面收到提示“数据发送失败”。请检查 ESP 模块的 Wi-Fi AP 是否正确开启，以及 AP 的相关配置，并确认已经输入正确的 AT 命令成功启用 web server。

使用浏览器进行 OTA 固件升级
------------------------------

简介
^^^^

浏览器打开 web server 的网页后，可以选择进入 OTA 升级页面，通过网页对 ESP 模块进行固件升级。

流程
^^^^

.. contents::
   :local:
   :depth: 1

打开 OTA 配置页面
""""""""""""""""""""

如图，点击网页右下角“OTA 升级”选项，打开 OTA 配置页面后，可以查看当前固件版本、AT Core 版本：

.. figure:: ../../_static/Web_server/web_brower_ota_config_page.png
   :align: center
   :alt: OTA 配置页面
   :figclass: align-center

   OTA 配置页面

**说明** 1：仅当浏览器连接 ESP 模块的AP，或者访问 OTA 配置页面的设备与 ESP 模块连接在同一个子网中时，才可以打开该配置界面。

**说明** 2：网页上显示的“当前固件版本”为当前用户编译的应用程序版本号，用户可通过 ``./build.py menuconfig`` --> ``Component config`` --> ``AT`` --> ``AT firmware version`` (参考 :doc:`../Compile_and_Develop/How_to_clone_project_and_compile_it`)更改该版本号，建立固件版本与应用程序的同步关系，以便于管理应用程序固件版本。

选择并发送新版固件
"""""""""""""""""""""

如图，点击页面中的“浏览”按钮，选择待发送的新版固件：

.. figure:: ../../_static/Web_server/web_brower_ota_chose_app.png
   :align: center
   :alt: 选择待发送的新版固件
   :figclass: align-center

   选择待发送的新版固件

**说明** 1：在发送新版固件之前，系统会对选择的固件进行检查。固件命名的后缀必须为.bin，且其大小不超过 2M。

通知固件发送结果
""""""""""""""""

如图，固件发送成功，将提示“升级成功”：

.. figure:: ../../_static/Web_server/web_brower_send_app_result.png
   :align: center
   :alt: 新版固件发送成功
   :figclass: align-center

   新版固件发送成功

同时，在串口端将收到如下信息：

::

    +WEBSERVERRSP:3      // 代表开始接收 OTA 固件数据
    +WEBSERVERRSP:4      // 代表成功接收 OTA 固件数据并且对数据的校验正确，此时 MCU 可以选择重启 ESP 设备，以应用新版本的固件

若接收的 OTA 固件数据校验失败，在串口端将收到如下信息：

::

    +WEBSERVERRSP:3      // 代表开始接收 OTA 固件数据
    +WEBSERVERRSP:5      // 代表接收的 OTA 固件数据校验失败，用户可以选择重新打开 OTA 配置界面，按照上述步骤进行 OTA 固件升级

使用微信小程序进行 Wi-Fi 配网
-------------------------------

简介
^^^^

微信小程序配网是通过微信小程序连接 ESP 设备创建的 AP，并通过微信小程序将需要连接的 AP 信息传输给 ESP 设备，ESP 设备通过这些信息连接到对应的 AP，并通知微信小程序配网结果的解决方案。

流程
^^^^

整个配网流程可以分为以下四步：

.. contents::
   :local:
   :depth: 1

配置 ESP 设备参数
"""""""""""""""""""""

为了让小程序连接 ESP 设备，ESP 设备需要配置成 AP + STA 模式， 并创建一个 WEB 服务器等待小程序连接，对应的 AT 命令如下：

#. 清除之前的配网信息，如果不清除配网信息，可能因为依然保留之前的配置信息从而导致 WEB 服务器无法创建。


   - Command
   
     ::
 
       AT+RESTORE

#. 配置 ESP 设备为 Station + SoftAP 模式。


   - Command
   
     ::
 
       AT+CWMODE=3

#. 设置 SoftAP 的 ssid 和 password（如设置默认连接 ssid 为 `pos_softap`，无密码的 Wi-Fi）。


   - Command
   
     ::
 
       AT+CWSAP="pos_softap","",11,0,3

#. 使能多连接。


   - Command
   
     ::
 
       AT+CIPMUX=1

#. 创建 WEB server，端口：80，最大连接时间：50 s（默认最大为 60 s）。


   - Command
   
     ::
 
       AT+WEBSERVER=1,80,25

加载微信小程序
""""""""""""""""

打开手机微信，扫描下面的二维码：

.. figure:: ../../_static/Web_server/web_wechat_applet_qr.png
   :align: center
   :alt: 获取小程序的二维码
   :figclass: align-center

   获取小程序的二维码

打开微信小程序，进入配网界面：

.. figure:: ../../_static/Web_server/web_wechat_open_applet.png
   :align: center
   :alt: 小程序配网界面
   :figclass: align-center

   小程序配网界面

目标 AP 选择
""""""""""""""""

加载微信小程序后，根据待连接的目标 AP，可将配网情况分为两种情况：  

1.待接入的目标 AP 为本机配网手机提供的热点。此时请选中微信小程序页面的“本机手机热点”选项框。

2.待接入的目标 AP 不是本机配网手机提供的热点，如路由器等 AP。此时请确保“本机手机热点”选项框未被选中。

执行配网
""""""""""""""""

待接入的目标 AP 不是本机配网手机
**************************************

这里以待接入的热点为路由器为例，介绍配网的过程：

1.打开手机 Wi-Fi，连接路由器：

.. figure:: ../../_static/Web_server/web_wechat_connect_rounter.png
   :align: center
   :alt: 配网设备连接拟接入的路由器
   :figclass: align-center

   配网设备连接拟接入的路由器

2.打开微信小程序，可以看到小程序页面已经自动显示当前路由器的 ssid 为"FAST_FWR310_02"。

.. figure:: ../../_static/Web_server/web_wechat_get_rounter_info.png
   :align: center
   :alt: 小程序获取拟接入的路由器信息
   :figclass: align-center

   小程序获取拟接入的路由器信息

注意：如果当前页面未显示已经连接的路由器的 ssid，请点击下图中的“重新进入小程序”，刷新当前页面：

.. figure:: ../../_static/Web_server/web_wechat_update_rounter_info.png
   :align: center
   :alt: 重新进入小程序
   :figclass: align-center

   重新进入小程序

3.输入路由器的 password 后，点击“开始配网”。

.. figure:: ../../_static/Web_server/web_wechat_rounter_connecting.png
   :align: center
   :alt: 小程序启动 ESP 模块连接路由器
   :figclass: align-center

   小程序启动 ESP 模块连接路由器

4.配网成功，小程序页面显示：

.. figure:: ../../_static/Web_server/web_wechat_rounter_connect_success.png
   :align: center
   :alt: 小程序配网成功界面
   :figclass: align-center

   小程序配网成功界面

同时，在串口端将收到如下信息：

::

    +WEBSERVERRSP:1      // 代表启动配网  
    WIFI CONNECTED       // 代表正在连接  
    WIFI GOT IP          // 连接成功并获取到 IP  
    +WEBSERVERRSP:2      // 代表小程序收到配网成功结果，此时可以释放 WEB 资源  

5.若配网失败，则小程序页面显示：

.. figure:: ../../_static/Web_server/web_wechat_rounter_connect_fail.png
   :align: center
   :alt: 小程序配网失败界面
   :figclass: align-center

   小程序配网失败界面

同时，在串口端将收到如下信息：

::

    +WEBSERVERRSP:1      // 代表启动配网，没有后续发起连接以及获取 IP 的信息，MCU 可以在收到该条消息后建立计时，若计时超时，则配网失败。

待接入的目标 AP 为本机配网手机
*********************************

如果正在配网的手机作为待接入 AP，则用户不需要输入 ssid，只需要输入本机的 AP 的 password，并根据提示及时打开手机 AP 即可（如果手机支持同时打开 Wi-Fi 和分享热点，也可提前打开手机 AP）。

1.选中微信小程序页面的“本机手机热点”选项框，输入本机热点的 password 后，点击“开始配网”。

.. figure:: ../../_static/Web_server/web_wechat_enter_local_password.png
   :align: center
   :alt: 输入本机 AP 的 password
   :figclass: align-center

   输入本机 AP 的 password

2.启动配网后，在收到提示“连接手机热点中”的提示后，请检查本机手机热点已经开启，此时 ESP 设备将自动扫描周围热点并发起连接。

.. figure:: ../../_static/Web_server/web_wechat_start_connect.png
   :align: center
   :alt: 开始连接本机 AP
   :figclass: align-center

   开始连接本机 AP

3.配网结果在小程序页面的显示以及串口端输出的数据与上述“待接入的目标 AP 不是本机配网手机”时的情况一样，请参考上文。

常见故障排除
^^^^^^^^^^^^
**说明** 1：配网页面收到提示“数据发送失败”。请检查 ESP 模块的 Wi-Fi AP 是否正确开启，以及 AP 的相关配置，并确认已经输入正确的 AT 命令成功启用 web server。

**说明** 2：配网页面收到提示“连接 AP 失败”。请检查配网设备的 Wi-Fi 连接功能是否打开，检查 ESP 模块的 Wi-Fi AP 是否正确开启，以及 AP 的 ssid、password 是否按上述步骤进行配置。

使用微信小程序进行 OTA 固件升级
---------------------------------
微信小程序支持在线完成 ESP 设备的固件升级，具体步骤与使用浏览器进行 OTA 固件升级类似，请参考上文。