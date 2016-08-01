# Matrix_Soc
  业务运维组管理平台v1版本
  Django+BootStrap+Highchart+Databales+Edior+layer+soc_wiki+ProcessOn+Tower+redis
 
 ```
【添加采集服务器SSH认证密钥串】
ssh_rsa/soc_web_id_rsa.pub
添加到 远程端服务器的：~/.ssh/authorized_keys

【加密脚本方法】
#shc -e 31/12/2010 -m "Please contact Gourav Joshi at Gmail dot com" -r -f configs.sh

【logging】
#定义日志输出格式
logging.basicConfig(level=logging.ERROR,
        format = '%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',
        datefmt = '%Y-%m-%d %H:%M:%S',
        filename = error_log,
        filemode = 'a')
try:
        conn = psycopg2.connect(database=db_name,user=db_user,password=db_pass,host=db_ip,port=5432)
        cursor = conn.cursor()
    except Exception,e:
        print e
        logging.error('数据库连接失败:%s' % e)
        return False

【开发日志 UpdateLog】
20160615 搭建Django实现模板层建立
20160620 调试模板与各子页面的CSS美观感
20160710 解决view.py 执行远程shell包含命令变量问题
20160719 历史性突破难点前后端数据交互ajax post    app_deploy.html
20160720 解决DataTables 前端表单启动事件&解决中文返回前端问题 shangyou.html & 远超传输调用脚本运行
20160721 开发增加logging模块,记录Django日志事件记录
20160722 开发增加soc_agent 采集模块get_shangyou_data.sh[ftp主动模式]采集汇总传送结果数据测试,升级采集版本,提高采集生成结果速度
20160725 研究使用redis方案,替换采集结果通过内网ftp传送&开发get_shangyou_data_redis.sh,研究redis哈希hash,数据的存取与所有域值输出获取&shangyou_opt.html 上游前端汇总显示页面>
的调试工作 ==完成DataTables ajax txt 上游接口程序展示.
20160726 升级服务端汇总方式采取循环监听策略soc_redis_all.sh(只要收集齐变汇总)&& 开发研究Datatables ajax 源方式增加JQuery事件按钮&& 解决前后端编码数据不符,split分割接口编号>
后,ajax post传回值为数组报错，暂搁置：utf-8 转gbk悬案(20160801找到解决办法)
20160727 将DataTables程序状态列用颜色进行区分&&开发增加对一行内容的查询更新操作,增加更新按钮,更新采集到新数据后回传,修改到汇总文件里 遗留问题:不能异步更新变化显示行,需要人>工执行加载获取采集，再刷新DataTables
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
```
  
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/222.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/333.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/444.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/555.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/666.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/777.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/888.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/aaa.jpg)
![img](https://github.com/Luolired/Matrix_Soc/blob/master/img/bbb.jpg)
