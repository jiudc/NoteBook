# Python

## Python基础

### 数据类型

- 整型
- 浮点
- 字符串
	- 字符串连接和复制
		- ‘Alice’ + ‘Bob’
		- ‘Alice’+ 42
		- ‘Alice’* 5
		- ‘Alice’*’Bob’

### 数学操作符

- **：指数
- %：取模
- //：整除
- /：除法
- *：乘法
- -：减法
- +：加法

### 比较操作符

- ==
- !=
- < 
- \>
- <=
- \>=

### 布尔操作符

- and
- or
- not

### 控制流

- if statement:
	  fun1
	 else:
	  fun2
- else:
- elseif statement:
- while statement:
	  fun3
- break
- continue
- for num in range(100)
	- range(0, 10, 2)：开始，停止，步长
- i = 0
	 while i<5:

### 异常处理

try except

```python
def spam(divideBy):
  try:
    return 42 / divideBy
  expect ZeroDivisionError:
    print(‘Error: Invalid argument.’)
```

### 列表

- spam = [‘cat’, ‘bat’, ‘rat’, ‘elephant’]
- spam[-1]
- spam[1:4]
- len(spam)
- spam[-1] =spam[0]
	- [1, 2, 3] + [‘A’, ‘B’]
	- [‘A’, ‘B’] * 3
	- del从列表中删除值,del spam[2]
- in 
- not in
- 多重赋值：x,y,z.c =spam，变量数目必须和列表数目一致，否则报错
- index：spam.index(‘cat’)
- append()，插入到末尾
- insert(1, ‘world’)
- spam.remove(‘cat’)，删除不存在会有ValueError错误，出现多次，只会删除第一次的值
- sort()，不能对既有数字又有字符串的列表排序

### 函数

- print()，sep分隔符，end结尾
	- print(‘Hello’, end=‘’)：print()函数自动在传入的字符串末尾加上换行符，end可换
	- print(‘cat’, ‘dog’, ‘mice’,sep=’,’：多个字符串值，print会自动用空格分隔，使用sep参数替换分隔符
- input()：等待用户在键盘上输入一些文本，并按下回车键
- len(str)：字符串str的字符个数
- str()，转变为字符串
- int()，转变为整数，或者取整
- float()，浮点数
- sys.exit()：程序终止或者退出
- import
	- import random
	- from random import *
- def hello()，定义函数
	- def hello(name):
		  print(‘Hello ’+ name)
		  return None
- global申明该变量为全局变量
- list(),转变为list，可变
- tuple()，转变为元组，不可变
- copy模块的copy()，复制列表或字典这样的可变值
- copy.deepcopy()，若复制的列表中包含列表，需要使用该函数
- random.shuffle()

### 元组数据类型

- eggs = (‘hello’, 42, 05)
- 不可变   

### 字典和结构化数据

- {key:value}
	- keys()
	- values()
	- items(
	- in:检查字典中是否存在键或值
- 字典不排序
- get()，检查键是否存在，若不存在则返回默认值
- setdefault()，如果键的值不存在，则设置
	- spam.setdefault(‘color’, ‘white’)
	- pprint.pprint(spam)，打印字典
	- print(pprint.pformat(spam))

### 字符串

不可变

- 按下标取值
- 切片
- for
- len()
- in
- not in
- “”：之间可以使用单引号
- 转义字符
	- \’
	- \”
	- \t
	- \n
- \\原始字符串：r’That is cao\’ cat’，忽略所有的转义字符
- ‘‘‘ ’’’，多行字符串，之间的所有引号、制表符或换行，都被认为字符串的一部分
- 多行注释: “““ ”””
- upper()
- lower()
- isupper(
- islower()
- isalpha()
- isalnum()
- isdecimal()
- isspace(
- istitle():仅包含以大写字母开头、后面都是小写字母的单词
- startswith()
- endswith()
- join(),’abc’.join([‘My’,’cat’])->’Myabccat’
- split(),spam.split(‘\n’)
- rjust(),对
- ljust()
	- ‘hello’.rjust(10)
	- ‘hello’.rjunst(10,’*’)
- center()
- strip()
- rstrip()
- lstrip()
- pyperclip模块copy()和paster()

### 正则表达式

- re
- regex = re.compile()
- mo=regex.search()
- mo.group()\mo.group(1)\mo.groups():使用括号分组
- |:匹配表达式的一个
	-  heroRegex = re.compile(r’Batman|Tina Fey’
	- mo1 = heroRegex.search(‘Batman and)
- ？：0或1次，可选匹配
- *：n次
- +：1次或more
- r’(ha){3,5}?’:？为表示非贪心，默认为贪心
- regex.findall():返回所有匹配，若分组，则返回元祖的列表
- \d,\D,\w,\W,\s(空格制表符或换行符),\S
- r’^Hello’：开头
- r’\d$’:以数字结束
- .：通配符，匹配除换行之外的所有字符.\.
- .*：任意文本*
- *通配符匹配换行：regex = re.compile(‘.*’, re.DOTALL)
- re.IGNORECASE、re.I:不区分大小写
- sub替换：regex.sub(r’\1*****’, String1)，\1表示分组1替换
- re.VERBOSE：告诉re.compile忽略正则表达式中的空白符和注释
- 如果需要同时使用re.IGNORECASE、re.DOTAILL、re.VERBOSE需要使用|

### 读写文件

- os，os.path.join():生成文件路径
- os.getcwd():获取当前工作目录
- os.chdir()：切换目录 
- os.makedirs()：创建新文件夹
- os.path.abspath(path):
- os.path.isabs(path)
- os.path.relpath(path,start):相对路径
- os.path.dirname(path):目录名称
- os.path.basename(path):基本名称
- os.path.getsize(path)：返回path参数中文件的字节数
- os.listdir(path):返回文件名字符串的列表
- os.path.exists(path)
- os.path.isfile(path)
- os.path.isdir(path)
- open(path),返回一个file对象
- file对象的read()
- readlines()：获取字符串的列表
- write()：写模式，添加模式
	- baconFile = open(‘bacon.txt’, ‘w’),写模式w会覆写原有文件
	- baconFile =open(‘bacom.txt’,’a’),添加模式

### shelve模块

- import shelve
- shelfFile = shelve.open(‘mydata’)
- shelfFile[‘cat’] = cats

运行后会生成文件mydata.bak\mydata.dat\mydata.dir

### 用pprint.pformat()函数保存变量

- pprint.pformat()提供了一个字符串，可以写入文本文件，如.py，此时可以使用import
	- fileObj.write(pprint.pformat(cat)+’\n’)

### 组织文件

- shutil模块
	- copy()
		- shutil.copy(‘c:\\spam.txt’, ‘c:\\delicious’)
	- move()
		- shutil.move(source, destination)
	- rmtree(path)：将删除path处的文件夹，包含所有文件和文件夹都别删除
	- os.unlink(path)：删除path处的文件
	- os.rmdir(path)：删除path处的文件夹，该文件夹必须为
-  send2trash模块
	- send2trash.send2trash，函数只是将文件送到垃圾箱
- 遍历目录树
	- os.walk(path)：在循环的每次迭代中，返回三个值：
		- for filderName, subfolders, filenames in os.walk(‘c:\\delicious’):
			- 当前文件夹的名称的字符串
			- 当前文件夹中子字符串的列表
			- 当前文件夹中文件的字符串的列表
- zipfile模块
	- 读取zip文件
		- exampleZip = zipfile.ZipFile(‘example.zip’)
		- exampleZip.namelist()
		- spamInfo = exampleZip.getinfo(‘spam.txt’)
		- spamInfo.file_size
		- spamInfo.compress_size
	- 从zip文件中解压缩
		- extractall(),参数用来指定解压缩的文件夹
	- 创建和添加到ZIP文件
		- 必须以写模式打开，newZip = zipfile.ZipFile(‘new.zip’, ‘w’)
		- newZip.write(‘spam.txt’, compress_type = zipfile.ZIP_DEFLATED)，指定压缩算法

### 1异常

- raise异常，raise Exception(‘This is the error message.’)
- 断言：禁用断言-O

### 日志

- import logging
	 logging.basicConfig(level=logging.DEBUG,format =’ %(asctime)s - %(levevlname)s - %(message)s’)
	 logging.debug(‘Start of program’)
- 保存到文件：logging.basicConfig(filename = ‘myProgramLog.txt’, level=logging.DEBUG,format =’ %(asctime)s - %(levevlname)s - %(message)s’)
- logging.disable(logging.CRITICAL),可以禁止日志
- 日志级别：
	- DEBUG
	- INFO
	- WARNING
	- ERROR
	- CRITICAL

## 从Web抓取信息

### webbrowser

- webbrowser.open(url)

### requests

- requests.get():下载一个网页

```python
path = r"C:\Users\Liudingchao\Src\Download"
os.chdir(os.path.join(path))
dir = os.getcwd();
print("my name:" + dir)
res = requests.get( 'https://gitee.com/it-ebooks/tutorialspoint-ebooks-zh/raw/master/TutorialsPoint%20AWT%20%E6%95%99%E7%A8%8B.epub')
res.raise_for_status()
playFile = open('test.jpg', 'wb')
for chunk in res.iter_content(100000):
    playFile.write(chunk)
playFile.close()
```

### Beautiful Soup

- 从HTML创建一个Beautiful Soup对象

```python
res = requests.get('http://it-ebooks.flygon.net/tutorialspoint')
res.raise_for_status()
searchSoup = bs4.BeautifulSoup(res.text)
```

- 从硬盘HTML文件创建

```python
exampleFile = open('example.html')
searchSoup = bs4.BeautifulSoup(exampleFile)
```

- select()方法寻找元素
	- soup.select(‘div’):所有名为<div>的元素
	- soup.select(‘#author’):带有id属性为author的元素
	- soup.select(‘.notice’):所有使用CSS class属性名为notice的元素
	- soup.select(‘div span’):所有在<div>元素之内的<span>元素
	- soup.select(‘div > span’):所有直接在<div>元素之内的<span>元素，中间没有其他元素
	- soup.select(‘input[name]’):所有名为<input>，并有一个name属性，其值无所谓的元素
	- soup.select(‘input[type=”button”]’):所有名为<input>，并有一个type属性，其值为button元素

### selenium

- 导入selenum需要使用from selenium import webdriver
- 需要安装chrome驱动，http://chromedriver.storage.googleapis.com/index.html

```python
browser = webdriver.Chrome()
browser.get(dstUrl)
```

- find_element_*:返回一个WebElement;_

- find_elements_*:返回WebElement_*对象的列表

- browser.find_element_by_class_name(name)：匹配CSS类的name元素

- browser.find_element_by_class_css_selector(selector)：匹配CSS的selector元素

- browser.find_element_by _id(id)；

- browser.find_element_by_link_text(text):完全匹配提供的text的<a>元素

- browser.find_element_by_partial_link_text (text):部分匹配

- browser.find_element_by__name(name)：匹配name属性值得元素

- browser.find_element_by_tag_name(name)：匹配标签name的元素（只有该方法大小写无关，<a>元素匹配‘a’和‘A’）

	**WebElement的属性和方法**

- tag_name:标签名，如<a>
- get_attribute(name):该元素name属性的值text:该元素内的文本
- clear():对于文本段或文本区域元素，清除其中输入的文本
- is_displayed():如果该元素可见，返回Tru
- is_enabled():对于输入元素，如果该元素启用，返回Tru
- is_selected():对于复选框或单选框元素，如果该元素被选中，返回True
- location:一个字典，包含键’x’和’y’，表示该元素在页面上的位置

**点击页面**

WebElement对象都有一个click()方法

**填写并提交表单**

- 向Web页面的文本字段发送，需要找到文本字段的<input>或<text>元素，然后调用send_keys()方法
- emailElem = browser.find_element_by_id(‘Email’)
	 emailElem.sendkeys(‘email@gamail.com’)
	 emailElem.submit()
- 在任何元素上调用submit()，都等同于点击该元素所在表单的submit按钮 

**发送特殊键**

- from selenium.webdriver.common.keys import Keys
- Keys.DOWN, UP, LEFT, RIGHT
- Keys.ENTER, RETURN;
- Keys.HOME, END, PAGE_DOWN, PAGE_UP
- Keys.ESCAPE, BACK_SPACE, DELETE
- Keys.F1, F2, …, F1
- Keys.TAB
	- htmlElem = browser.find_element_by_tag_name(‘html’)
		 htmlElem.send_keys(Keys.END)
		 htmlElem.send_keys(Keys.HOME)

**点击浏览器按钮**

- browser.back():后退
- browser.forward()：前进
- browser.refresh():刷新
- browser.quit():关闭窗口

## 处理Excel

### 读取Excel

- import openpyxl
- wb=openpyxl.laod_workbok(“example.xlsx”)：返回一个workbook对象
- wb.get_sheet_name()：取得工作簿中所有表名的列表
- sheet=wb.get_sheet_by_name(‘sheet3’)，depressed，现变更为wb[‘sheet3’]
- sheet=wb.get_active_sheet()：获取活动的workbook
- sheet.title：获得名称
- c=sheet[‘A1’]：获取Cell对象
- c=sheet.cell(row=1, column=2)：获取Cell，第一行第一列为1，非零
	- c.value
	- c.row
	- c.column
	- c.coordinate
- sheet.get_highest_row()/sheet.get_highest_column()：获取表的大小
- 列字母和数字之间的转换：
- from openpyxl.cell import get_column_letter, column_index_from_string:转换
- 从表中取得行和列：sheet[‘A1’:’C3’]

```python
for rowsOfCellObjects in activeSheet['A1':'C3']:
    for cellObj in rowsOfCellObjects:
        print(cellObj.coordinate, cellObj.value)
```

### 写入Excel

- wb=openpyx.Workbook():新建
- wb.create_sheet()：<Worksheet “sheet1”>，返回一个新的Worksheet对象，名为SheetX
- wb.create_sheet(index=0, title= “First Sheet” )：<Worksheet “First Sheet”>
- wb.remove_sheet(wb.get_sheet_by_name(“sheet1”))：接收一个Worksheet

### 设置单元格的字体风格

设置字体风格

```python
wb = openpyxl.Workbook()
sheet = wb["Sheet"]
italic24Font = Font(size=24, italic=True)
sheet['A1'].font = italic24Font
sheet['A1'] = "Hello World"
wb.save("Font.xlsx")
```

### Font对象

- Font()函数，关键字参数
	- name：字体名称
	- size：大小点数
	- bold：True表示粗体
	- italic：True表示斜体

### 公式

- sheet[‘B9’] = ‘=sum(b1:b8)’：直接赋值
- 如果调用load_workbook()带有data_only=True，则显示公式的结果，而不是文本

### 调整行和列

- 设置行高和列宽
- sheet.row_dimensions[1].height = 70；
- sheet.column_dimensions[‘B’].width = 20；

### 合并单元格

- sheet.merge_cells(‘A1:D3’)，若要设置这个值，设置这个组合单元格左上角的单元格的值
- sheet.unmerge_cells(‘A1:D3’)，拆分单元格

### 冻结窗格

sheet.freeze_pans =’C2’，冻结单元格上边的所有行和左边的所有列，本身不会冻结

### 图表

append可用于一次添加多行数据

```python
wb = Workbook(write_only=True)
ws = wb.create_sheet()
rows = [
    ('Number', 'Batch 1', 'Batch 2'),
    (2, 10, 30),
    (3, 40, 60),
    (4, 50, 70),
    (5, 20, 10),
    (6, 10, 40),
    (7, 50, 30),
]

for row in rows:
    ws.append(row)
```

 https://openpyxl.readthedocs.io/en/stable/index.html

## 保持时间、计划任务和启动程序

### time模块

- time.time()：从Unix纪元(1970年1月1日0点)那一刻的秒数
- time.sleep()：暂停

### 四舍五入

- round(time.time(), 2：时间处理

### datetime模块

- KeyboardInterrupt：当用户按下Ctrl+C则抛出该异常
- datetime.datetime.now()：当前时间
- dt = datetime.datetime(2015,10,21,16,59,0)：生成特定时刻datetime对象
- dt.year\month\day\hour\minute\second：属性
- datetime.datetime.fromtimestamp(time.time())与now()一样
- datetime可用比较操作符
- timedelta数据类型
	- delta = datetime.timedalta(days=11, hous=10)，关键字参数有weeks\days\hours\minutes\seconds\milliseconds\microseconds
-  这些数字分别保存在days\seconds\microseconds属性中
- delta.total_seconds()获取总秒数
- datetime对象转换为字符串，strftime()
	- oct21st=datetime.datetime(2015,10,21,16,29,0)
		 oct21st.strftime(‘%Y/%m/%d%H%M:%S’)
		- %Y：2014
		- %y：14
		- %m：09
		- %B：Novermber
		- %b：Nov
		- %d：31
		- %j：366
		- %w：0~6（周日~周六）
		- %A：Monday
		- %a：Mon
		- %H：00~23
		- %I：01~12
		- %M：分，00~59
		- %S：秒，00~59
		- %p：AM、PM
		- %%：%字符
	- 字符串转换成datetime，strptime()，p表示parse
		- datetime.datetime.strptime(‘October 21, 2015’, ‘%B %d,%Y’);

## 多线程

- threadObj = threading.Thread(target=takeAnap);takeANap()不带括号，表示这是将函数本身作为参数
- threadObj.start()
- 传递给新线程函数，使用args=[‘Cats’,’Dogs’]传递常规参数和kwargs={‘sep’: ‘ &’ }用于关键字参数。

## 从Python启动其他程序

- subprocess模块的Popen()函数
- subprocess.Popen(‘C:\\Windows\\System32\\calc.exe’)；
- 返回Popen对象
	- poll()，若该进程仍在运行，则返回None，若已经终止，无错终止返回0，其他非0
	- wait()，将阻塞
	- 传递命令行参数，sys.argv
	- subprocess.Popen([‘c:\\Windows\\notepad.exe’, ‘c:\\hello.txt’])；
- 使用默认的应用程序打开文件：
	- subprocess.Popen([‘start’, ‘hello.txt’],shell=True)；

## 发送电子邮件和短信

### SMTP

- 连接SMTP服务器
	- smtpObj = smtplib.SMTP(‘smtp.gmail.com’, 587)：TLS（Transport Layer Security，传输层安全协议）
	- smtpObj = smtplib.SMTP(‘smtp.163.com’, 465)：SSL（Secure Socket Layer，安全套接字层）
- 发送SMTP的“Hello”消息
	- smtpObj.ehlo()
- 开始TLS加密，SSL无需此步
	- smtpObj.starttls()
- 登录到SMTP服务器
	- stmpObj.login(“email”,“password”)：登
	-  如果登录126，虽然密码正确，smtplib.SMTPAuthenticationError: (535, b'Error: authentication failed')，需要开启授权码                    
-  发送电子邮件

```python
smtpObj.sendmail("ldc10@126.com", "908009280@qq.com", "Subject: Test for Python!\nDear Alice”)
                 sendMail = 'ldc10@126.com'
receiveMail = '908009280@qq.com'
msg = MIMEText('<html><h1>大佬好！</h1></html>', 'html', 'utf-8')
msg['Subject'] = Header("test_email", 'utf-8')
msg['from'] = sendMail
msg['to'] = receiveMail
res = smtpObj.sendmail(sendMail, receiveMail, msg.as_string())
```

-  从SMTP服务器断开
	- smtpObj.quit()

### IMAP

- 连接到IMAP服务器，创建IMAPClient对象，大多数需要SSL加密，SSL=TRUE

```python
imapObj = imapclient.IMAPClient("imap.126.com", ssl=True)
```

- 登录到IMAP服务器

```python
imapObj.login(dstMail, passWord)
```

- 查看所有的文件夹(IMAP可以支持创建文件夹)

```python
folders = imapObj.list_folders()
```

- 选择文件夹
	- select_folder()，不输入参数默认为INBOX，可加参数readonly=True
		 'NO', [b'SELECT Unsafe Login. Please contact kefu@188.com for help']
	- 需要登录http://config.mail.126.com/settings/imap/login.jsp?uid=ldc10@126.com处理

```python
imapObj.select_folder('INBOX', readonly=True)
```

- 搜索，serach()，返回邮件UID
	- ALL
	- BEFORE data, ON, SINCE
	- SUBJECT string, BODY, TEXT, FROM, TO, CC(抄送), BCC(密件抄送)
	- SEEN(已看标记), UNSEEN
	- ANSWERED, UNANSWERED
	- DELETED, UNDELETED
	- DRAFT, UNDRAFT(草稿)
	- FLAGGED, UNFLAGGED(重要或紧急)
	- LARGGER N, SMALLLER
	- NOT search-key
	- OR search-key1 search-key2

```python
UIDS=imapObj.search(['ALL'])
```

- 获取具体内容

```python
rawMessages = imapObj.fetch(UIDS, ['BODY[]'])
```

- 获取得数据如下，在’BODY[]’中的内容为RFC822，是专为IMAP服务器读取为设计的
- 如果想要把获取时标记为已读，需要使用readonly=False
- 大小限制
	- imaplib._MAXLINE = 1000000
- 从原始消息中获取电子邮件地址
- pip install pyzmail，此处有bug，换下列命令 
- easy_install pyzmail
- 创建PyzMessage对象

```python
message = pyzmail.PyzMessage.factory(rawMessages[1320241627][b'BODY[]'])
```

- 获取主题，地址等

```python
subject = message.get_address(“from”)；to、cc、bcc
```

- 从原始消息中获取正文

	- 文本

		```python
		if message.text_part:
		    messageTxt = message.text_part.get_payload().decode(message.text_part.charset)
		```

	- Html

		```python
		 if message.html_part:
		    messageHtml = message.html_part.get_payload().decode(message.html_part.charset)u  
		```

- 删除电子邮件
	- imapObj.delete_message(UIDs)
	- imapObj.expunge()
- 从IMAP服务器断开
	
	- imapObj.logout()