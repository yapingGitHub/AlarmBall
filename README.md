# AlarmBall
报警球Android客户端设计文档

1	功能概要
	下发下装手机号码命令
	下发设置报警球报警距离命令
	下发查询报警球信息命令
	下发打开，关闭报警球探头命令
	可添加，删除，编辑报警球信息，存储在数据库中
	监听报警球返回短信，并解析短信内容，反馈在界面上
	保存发送和接收的信息概要在客户端中供日后查询
	检测服务器更新版本
2	界面
2.1	启动界面
	Logo淡入2秒
	界面淡出
2.2	启动界面2
	界面淡入
	报警球旋转2秒
	界面淡出
2.3	主界面
	除了首次启动程序，以后每次启动程序都进入本界面
	上方布局显示选择报警球信息
	“报警球设定”滑动进入报警球状态界面
	5个按钮对应5个下发命令
	按钮按下后图片背景变化
	每次进入界面都从SharedPreferences中取出上次设置的后台手机号码，报警球信息，如果取出的号码已不在数据库中，显示未选择
	界面启动时启动本界面短信监听器，界面退出时暂停监听器
	检测服务器端是否有更新版本
2.3.1	报警球设定按钮
	打开报警球状态界面
2.3.2	下载手机号按钮
	弹出后台手机号设定界面
2.3.3	设定报警距离按钮
	弹出报警距离设定界面
	将报警距离写入该球数据库
2.3.4	查询报警球信息按钮
	发送查询短信
2.3.5	打开，关闭探头
	发送命令短信
2.3.6	Menu菜单
	历史数据-打开历史数据查询界面，可查询收发短信，报警信息
2.4	报警球设定界面
	显示报警球号码，位置，状态，报警距离
	单击报警球信息按钮，界面滑入
	每次打开界面读取SQLite更新ListView内容
	单击ListView项目返回“主界面”，将选择项目信息赋给主界面报警球信息，并显示，选择信息存储在SharedPreferences中。
	界面通过返回按钮淡出返回“主界面”
	通过menu按钮添加报警球，位置限制在5个汉字内，报警球号码可从联系人中取
	添加报警球前遍历SQLite是否有该号码的报警球
	通过长按ListView项目删除，修改报警球，或查看此报警球详细信息
	每次对报警球信息更改后更新ListView 
	界面启动时启动本界面短信监听器，界面退出时暂停监听器
	界面滑出
2.5	报警球详细信息界面
2.5.1	基本信息
	号码，位置，当前状态
2.5.2	历史数据
日期，操作或反馈
	接收短信历史：报警及安全
	指令发送历史
2.6	历史信息查询界面
主界面按menu进入，可按日期查询历史，设定日期保存在Sharedpreferences中，结束日期初始值每天更新为当天值
2.6.1	短信发送查询
	Listview可上下左右滑动
	发送日期，发送对象号码，发送内容（翻译），报警球位置
	如果发送失败不存储
2.6.2	短信接收查询
	Listview可上下左右滑动
	接收日期，接收对象号码，报警球位置，接收内容（翻译）
2.6.3	报警信息查询
	Listview可上下左右滑动
	报警日期，报警球位置，报警球号码，报警类型（翻译）
2.7	后台手机号设定界面		
	单击下载手机号码按钮进入，界面弹出。
	对话框显示最近设置后台手机号
	分仅存储和下装后台（发送短信）两种模式
	存储用户输入的后台手机号码到SharedPreferences中，并更新主界面按钮文字。
2.8	报警距离设定界面
	单击报警距离设定按钮进入，界面弹出
	对话框显示最近设置报警距离
	分仅存储和下装后台（发送短信）两种模式
	最多1位小数
	如果选择发送，将用户输入值*100后转成文字发出
	用户输入值在1-8之间，超出Toast提示
	存储用户输入的后台手机号码到SharedPreferences中，并更新主界面按钮文字。

3	通知
3.1	Toast
当程序在前台运行时使用，显示同时带有声音辅助提示
	已添加报警球信息：添加报警球信息成功
	已删除报警球信息：删除报警球信息成功
	已修改报警球信息：修改报警球信息成功
	请输入报警球号码和位置：修改或添加报警球未输入报警球号码或位置
	此号码报警球已存在：修改或添加的报警球已存在于数据库中
	已设置后台号码：设置后台号码成功
	已设置报警距离：设置报警距离成功
	指令发送成功：指令发送成功（发送音）
	指令发送失败：指令发送失败
	位置+报警球+设定成功：报警球回复OK！（安全音）
	位置+报警球+新报警：报警球回复新报警短信（报警音）
	位置+报警球+持续报警：报警球回复持续报警短信（报警音）
	位置+报警球+安全：报警球回复恢复安全短信（安全音）
3.2	Notification
当程序在后台运行时使用
	新报警
	持续报警
	安全：不通知，将该报警球所有报警Notification清除
4	后台进程
4.1	程序状态监视器
监视客户端数据，获取用户使用习惯。
4.1.1	将用户使用程序状态数据通过wifi回传至公司。
	程序使用次数
	报警球报警次数
	报警球添加个数
	各模块使用次数
4.2	短信监听器
4.2.1	系统级（优先级900）
	启动程序时启动，不关闭
	接收系统短信广播
	短信发出后使用监听短信是否发送成功，并提示
	把发送的信息，存入数据库
	发送失败的短信，从数据库删除
	收到报警球返回的OK短信时，提示
	收到报警球返回的短信，存入数据库
	更新报警距离，报警球状态字段
	如果收到OK！短信（设定成功）
不在程序中：不通知
在程序中：Toast通知，发出声音
	如果收到安全短信
不在程序中：不通知，清除对应报警球已有notification
在程序中：Toast通知，发出声音
	如果收到报警短信（新报警和持续报警）
不在程序中：通知栏里通知，发出声音
在程序中：Toast通知，发出声音 
	从通知栏可直接进入报警历史查询界面
	如果已有多个notification通知，点击任意通知，其它通知消失
4.2.2	报警球设定内部类（优先级700）
	打开界面时启动，退出界面时关闭
	刷新ListView
4.2.3	主界面内部类（优先级600）
	打开界面时启动，退出界面时关闭
	接收系统短信广播
	更新主界面报警球状态
	更新设定距离显示文字
	更新设定距离弹出框默认文字
4.2.4	系统级（优先级300）
	最后一个执行的监听器
	如果短信为报警球所发，拦截短信
5	程序存储字段
5.1	Sharedpreferences（临时存储）
	报警球信息——手机号，位置，状态，报警距离
	下装手机号信息
	历史查询-开始日期
	历史查询-结束日期
	程序累计启动次数
	选择以后更新时程序累计启动次数
	是否选择以后更新
5.2	SQLite（数据库）
	报警球信息—可查看，删除，编辑，添加
	报警信息—接收短信的一类，自动添加，只可查看
	发送短信—自动添加，只可查看
	接收短信—自动添加，只可查看
6	问题
6.1	短信拦截
	如果Ballinfo内部监听器先执行，那么收到短信后，界面更新在数据库更新之前，界面更新将没有效果。
	部分运营商短信前缀有+86标示，所以收到的短信只取后11位作为发送方号码。
	如果报警球号码发送非正常短信，不拦截，正常显示，并不进入报警信息判定
	Android7.0后短信不能再被直接拦截。因为只能授权一个进程接收短信广播。
如果本APP接收了短信，那么就会造成系统不能接收短信。
如果只是提高优先级不能影响系统接收到短信。

