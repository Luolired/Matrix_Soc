# Matrix_Soc

## 业务运维组管理平台v2版本 【自研】
    
### 前端：Matrix模板+BootStrap+layer+Highchart+Datatables
### 业务平台：Django+Shell+Ansible agent+Server+crontab(==>celery) 
### 第三方工具模块：Edior+soc_wiki+ProcessOn+Tower+Supervisor+cesi（==>淘汰自己写任务工单管理系统）
### 数据存储：mysql+redis(哈希映射临时存储)

##开源计划（待离职）

![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/222.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/333.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/444.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/Luolired.png)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/555.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/666.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/777.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/888.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/aaa.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/bbb.jpg)

 ```
【开发日志 UpdateLog】
20160615 搭建Django实现模板层建立
20160620 调试模板与各子页面的CSS美观感
20160710 解决view.py 执行远程shell包含命令变量问题
20160719 历史性突破难点前后端数据交互ajax post    app_deploy.html
20160720 解决DataTables 前端表单启动事件&解决中文返回前端问题 shangyou.html & 远超传输调用脚本运行
20160721 开发增加logging模块,记录Django日志事件记录
20160722 开发增加soc_agent 采集模块get_shangyou_data.sh[ftp主动模式]采集汇总传送结果数据测试,升级采集版本,提高采集生成结果速度
20160725 研究使用redis方案,替换采集结果通过内网ftp传送&开发get_shangyou_data_redis.sh,研究redis哈希hash,数据的存取与所有域值输出获取&shangyou_opt.html 上游前端汇总显示页面的调试工作 ==完成DataTables ajax txt 上游接口程序展示.
20160726 升级服务端汇总方式采取循环监听策略soc_redis_all.sh(只要收集齐变汇总)&& 开发研究Datatables ajax 源方式增加JQuery事件按钮&& 解决前后端编码数据不符,split分割接口编号后,ajax post传回值为数组报错，暂搁置：utf-8 转gbk悬案(20160801找到解决办法)
20160727 将DataTables程序状态列用颜色进行区分&&开发增加对一行内容的查询更新操作,增加更新按钮,更新采集到新数据后回传,修改到汇总文件里 遗留问题:不能异步更新变化显示行,需要人工执行加载获取采集，再刷新DataTables
20160728 实现启动后,Databales能重新进行加载。刷新指定数据表格 Error:oTable.fnSettings is not a function oTable.fnReloadAjax is not a function [搁置,无法解决插件报错],最后使用:ajax.reload()解决。方法:table.ajax.reload( null, false ); // 刷新表格数据，分页信息不会重置 至此解决:动作按钮执行后,数据不仅反映结果弹窗而且还加载新的DataTables [搁置bug] 操作记录bug非当前页面用户,都是为最后一个登陆系统的人名
20160729 解决手机端,线上乱码问题,支持手机管理界面排版。
20160801 加载数据的革新。从redis直接导出汇总数据,解决datatables,加载数据依赖txt形式的复杂度。      
	 1.//"sAjaxSource": "data/shangyou_appdata_hash.txt" ===> sAjaxSource": "shangyou-optFun",
	 2.gbk转utf-8数据处理
		(status, output) = commands.getstatusoutput('redis-cli HVALS shangyou_appdata_hash')
                result=output.decode('gbk','ignore').encode('utf-8','ignore')
                #去重最后一个字符
                result=result[:-2]
                data="{\"data\":["+result+"]}"
                return HttpResponse(data)
	 3.实现方案一：
		1.采集端采集端完成后存储在redis 哈希表中 【每5分钟采集】【覆盖哈希】
		2.单独用一个脚本进行哈希表数据的导出，汇总 soc_redis_all.sh 【常驻进程满三汇总脚本，汇总完毕后删除】
		3.视图层view只判断post操作数据类型　Ｔ前端层之间读取汇总后结果文件shangyou_appdata_hash．ｔｘｔ
	 4. 实现方案二：
		１.1.采集端采集端完成后存储在redis 哈希表中 【每5分钟采集】【覆盖哈希】
		２.直接在视图层ｖｉｅｗ层判断ｇｅｔ　请求后进行从ｒｅｄｉｓ导出汇总

		比较：方案一　只修改单独程序时可以单独查询到的结果修改shangyou_appdata_hash．ｔｘｔ
		方案二：简单，无须关注shangyou_appdata_hash．ｔｘｔ生成结果，ａｊａｘ无法访问问题
		但每更新一行刷新时，都需要进行　这台服务器数据重新采集会ｒｅｄｉｓ【无法实现修改ｒｅｄｉｓ哈希表里的其中一行数据哦＼局限在采集点要重新刷新】
20160802 
	实现Django站点管理，管理用户组、用户名、修改密码功能。升级使用django_admin_bootstrapped,达到手机响应式支持。美化前端CSS框架base.html,修改Head,增加Matrix Logo.
	导航条布局修改,修改站点管理登录界面达到和站点面板页面登录一致
20160803 
	站点管理界面增加页面右侧浮动固定层返回顶部按钮带二维码展示功能 & 取消资产管理系统的认证,直接使用django is_authenticated & 全局变量名取登录名显示【搁置,中文全程名更美观,待实现】 [待解决:导航栏子菜单选中其一后仍需要保留原子菜单列,而不是撤销]
20160804
	完成重大突破,实现Django 用户权限管理.关键要点： user.get_all_permissions()  {% if perms.Soc_Ops_App.woker_report %} 增加 Soc_Ops_App.worker_report 等自定义四种角色权限 & 权限测试。
20160805
	解决前端报错bug:Uncaught TypeError: undefined is not a function $('.tip').tooltip();  jquery冲突:<script src="/js/jquery.min.js"></script> && 解决下拉子菜单选中后又弹回刷新问题，达到用户选择后仍然保持同级子目录 && 完成目录架构url 建立目录分门别类而且在view层 index直接一处定义修改子菜单名称路径 && 解决 主面板左上角logo 无法url跳转问题，解决注释z-index,至此 Matrix-Soc 完成 1.0版本最终发布
20160820
    重新对数据库字段表重新设计,表与表关联通过id进行链接.show create table yunwei_program;
<br/>
20160822
    因为表字段的更改，重新修正js与前端html字段名称
<br/>
20160825
    改善用户体验，在多表查询关系下,可以选择列段进行隐藏功能
<br/>
20160901
    增加导航链接组,方便跳转到其他子后台管理系统如Nagios\Cacti\007后台\进程管理系统
<br/>
20161009
    由于工作时间忙于Supervisor与keepalived高可用实施，重新开始开发
    1.解决主页home/dashboard.html打开加载缓慢的问题(注释获取<!--link href='/css/fonts.useso.css' rel='stylesheet' type='text/css'-->)
<br/>
    20161013
    增加工具箱功能 可以使用yhbf 输入字符串进行加解密功能。涉及 HTML+POST+AJAX+VIEW。成功解决多层目录CSRF 403 和500报错，为表单的提交和返回结果垫>
底模板基础。原因在于多层目录需要直接指定路径eg:action="/tools_data/tools_yhbf" url:"/tools_data/tools_yhbf", url(r'^tools_data/','Soc_Ops_App.views.tools_data')
<br/>
	20161031
    mysql> show create table yunwei_program;解决存储数据select类型数据库存储为tinyint类型,前端需要更换对应列表进行转义成对应前端显示功能。
    开发解决DataTables mDataProp 属性"mRender": function 判断条件,同时解决 数据加载后无法刷新bug,将//oTable.fnReloadAjax(oTable.fnSettings());转>换成: oTable.ajax.reload(oTable.settings()); 数据增删改后，立即刷新返回
<br/>
20161107
    开发解决前端【开发日志】输出文本内容按天分隔
<br/>
20161114
    顺利解决采集器Agent_007ka.py commands.getoutput("curl http://members.3322.org/dyndns/getip").split('\n')[-1] 获取外网ip问题
    重大更新解决多个ip同时执行采集汇总插入数据库问题。
<br/>
20161115
    开发处理主机索引页面500无法新增修改问题，原因为:前端id组合号写错,导致无法批准组合成串;成功实现批量ip的数据采集,主机索引页面成功可以开启交付
<br/>
20161122
    开发应用查询功能页面三大功能1.主机ip与主机列表显示关联 2.修改应用程序直接关联后端数据表 3.新增应用程序自动转换ip为主机id insert进表 【关联表>增删改开发】
    4.解决python datetime.datetime is not JSON serializable 报错问题解决5.修改前端可选列端显示隐藏功能美化前端颜色辨别程序状态
<br/>
20161123
    开发Datables数据导出功能失败(按键无法触发execlFlash动作),项目搁浅,恭喜 终于解决文件,可以支持下载导出功能。问题原因为:a.repalce is not funciton。解决方案更新dataTables.buttons.js和buttons.flash.js
20161130
    开发应用程序列表新增修改应用程序列表,实现跨表增删改view.py\Server_007ka.py
<br/>
20161206
    处理采集时programrun过长问题,优化采集的数输出,以及准备建立crontab定时任务的采集,结果Server_007ka.py虽然成功调用,但是没有被执行成功,原因不明
<br/>
20161207
    解决ansible在定时任务中无法被执行的原因.有两点需要注意1.private_key_file在插件或配置中需要定义 2.id_rsa需要无密码,id_rsa.pub在采集angent需要3
.稍微注意ansible.cfg hosts配置
<br/>
20161215
    开发app_search.html yunwei_program表,每个应用程序后面增加4个按钮,刷新、启动、停止、重启程序.实现程序操作管理
    a.解决aoColumns与columnDefs一起配置空白问题,最后解决调试为:"bAutoWidth": false,"mDataProp": null,headerCells[i].style.width
    b.解决buttons click动作无执行结果问题,解决table not define,调试后增加的4个按钮必须是在initTable()函数里,click动作
    c.解决全局变量登录用户名 gloal name,没有被定义,解决为调试赋值成新的true_name
<br/>
20161220
    修改整体前端UI字体,T.html模板里加入layui.css,使整体字体美观效果更突出显示,提高用户体验
    1.增加日期时间组件文档 - layui.laydate 选择btime时间.可以选择日期和具体时间,但因layDate官方近期将会一次大的改写,暂研究到此
<br/>
20161221
    1.开发onlinechart.html 代理商进单数排名 Tables功能和Highchart饼图,可以实现自定义日期查询和TOP数选择2.解决Python Python to JSON Serialization fails on Decimal问题
    20161222
    1.开发onlinechart.html 三系统订单笔数概况 可下钻的柱状图,可以实现查询三系统前一个小时的订单概略以及三个系统各自分布状态,提供了非常好的Httresponse(date)多条数据，以及highchart data数据的组合,待继续完成:其他汇总列+时间控件选择具体时间进度情况。
<br/>
20161223
    1.开发onlinechart.html 重点关注代理商24H进单概况对比 Full Width前24小时的前后两天的对比，解决时间问题的bug,提高容错性
<br/>
20161224
    1.解决当前事情02分数据还在采集的报错bug,开发重点关注代理商24H进单概况对比,可自定义选择代理商对比功能
<br/>
20170208
    1.进行大规模重新编译,加入任务管理系统与用户权限管理,逐步淘汰使用django默认用户权限管理模块,前端模块的【任务管理】与【项目管理】库表设计与前端增删改查的实现，解决date显示问题
<br/>
20170209
    1.实现了 用户管理和用户组管理,前端增删改.与部门ID关联表显示展现
```
