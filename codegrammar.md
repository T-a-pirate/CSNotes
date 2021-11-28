## 调试
方法1：断言(assert)
``` python
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n

def main():
    foo('0')
```
assert的意思是，表达式n != 0应该是True，否则，根据程序运行的逻辑，后面的代码肯定会出错。

如果断言失败，assert语句本身就会抛出AssertionError：
``` python

$ python err.py
Traceback (most recent call last):
  ...
AssertionError: n is zero!
```
##  logging
``` python
import logging
logging.basicConfig(level=logging.INFO)
//允许你指定记录信息的级别，有debug，info，warning，error等几个级别，当我们指定level=INFO时，logging.debug就不起作用了。同理，指定level=WARNING后，debug和info就不起作用了。

s = '0'
n = int(s)
logging.info('n = %d' % n)
print(10 / n)
```
##  pdb  pdb.set_trace()
``` python
# err.py
s = '0'
n = int(s)
print(10 / n)
``` 
``` python
python -m pdb err.py
```
命令l来查看代码
输入命令n可以单步执行代码
输入命令 p 变量名 来查看变量
(Pdb) p s
'0'
q结束调试
## pdb.set_trace()
需要import pdb，然后，在可能出错的地方放一个pdb.set_trace()，就可以设置一个断点
```python
# err.py
import pdb

s = '0'
n = int(s)
pdb.set_trace() # 运行到这里会自动暂停
print(10 / n)
```
程序会自动在pdb.set_trace()暂停并进入pdb调试环境，可以用命令p查看变量，或者用命令c继续运行

##  单元测试
“测试驱动开发”（TDD：Test-Driven Development）
确保一个程序模块的行为符合我们设计的测试
编写单元测试时，我们需要编写一个测试类，从unittest.TestCase继承。
``` python
class TestDict(unittest.TestCase):
```
以test开头的方法就是测试方法，不以test开头的方法不被认为是测试方法，测试的时候不会被执行。对每一类测试都需要编写一个test_xxx()方法.
调用这些方法就可以断言输出是否是我们所期望的。最常用的断言就是assertEqual()：

``` python
self.assertEqual(abs(-1), 1) # 断言函数返回的结果与1相等
```
###单元测试
1.通过参数-m unittest直接运行单元测试
``` python 
 python -m unittest mydict_test
```
2.在mydict_test.py的最后加上两行代码：
``` python 
if __name__ == '__main__':
    unittest.main()
```
##文档测试
python内置“文档测试(dectest)"模块可以直接提取出注释中的代码并执行测试

---

# IO编程
   1 .基本概念：input， output，stream
   2 .存在问题：输入和接收速度不匹配
   3 .解决方法：同步、异步(回调--好了叫我，轮询---好了没...好了没)
   4 .收获新知：编程语言都会把操作系统提供的低级C接口封装起来方便使用

## 文件读写
读文件：open()函数
``` python 
f = open('/Users/michael/test.txt', 'r')
```
文件不存在抛出IOError错误
文件成功打开用read()方法读取文件内容,read()一次读写， read(size)读取size大小的文件，防止文件过大导致内存溢出。
``` python
f.read()
```
文件读写时都有可能产生IOError，一旦出错，后面的f.close()就不会调用。,故用try ... finally实现：
``` python 
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```
可用 with 语句实现,不用调用f.close()
``` python
with open('/path/to/file', 'r') as f:
    print(f.read())
```
读取二进制文件用rb
``` python
f = open('/Users/michael/test.jpg', 'rb')
```
读取非UTF-8的文本文件，传入encoding参数，如读取gbk的文件：
``` python 
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
>>> f.read()
```
如果存在非法的字符，使用errors参数
``` python
f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```
### 写入
传入标识符 w 或者 wb
``` python
f = open('/Users/michael/test.txt', 'w')
f.write('Hello, world!')
f.close()
```
写入完成后调用f.close()关闭文件
用with写入不用手动关闭文件
``` python
with open('/Users/michael/test.txt', 'w') as f:
    f.write('Hello, world!')
```
python里的文件路径：
``` python
1. r'c:\Users\'
2. 'c:\\Users\\'
3. 'c:/Users/'
```
## StringIO 和 BytesIO
读写str SrtringIO
``` python 
>>> from io import StringIO
>>> f = StringIO()
>>> f.write('hello')
5
>>> f.write(' ')
1
>>> f.write('world!')
6
>>> print(f.getvalue())
hello world!
```
读写二进制 BytesIO()
``` python 
>>> from io import BytesIO
>>> f = BytesIO()
>>> f.write('中文'.encode('utf-8'))
6
>>> print(f.getvalue())
b'\xe4\xb8\xad\xe6\x96\x87'
```
## 操作文件和目录
``` python 
import os
os.name #操作系统类型
os.environ.get('key')获取key(一个环境变量)的值
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/Users/michael'
# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
>>> os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
# 然后创建一个目录:
>>> os.mkdir('/Users/michael/testdir')
# 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
```
合并路径：os.path.join()函数
路径拆分：os.path.split()函数
``` python 
>>> os.path.split('/Users/michael/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')
# 对文件重命名:
>>> os.rename('test.txt', 'test.py')
# 删掉文件:
>>> os.remove('test.py')
#列出当前目录下的所有目录
[x for x in os.listdir('.') if os.path.isdir(x)]
#列出所有的.py文件
[x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
```
## 序列化
把变量从内存中变成可存储或传输的过程称之为序列化。picking
``` python
>>> import pickle
>>> d = dict(name='Bob', age=20, score=88)
>>> pickle.dumps(d)
b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'
```
### JSON

``` Python
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}
```
# 多进程
fork()系统调用，仅unix/linux
跨平台多线程multiprocessing模块提供了一个Process类来代表一个进程对象(windows也可用)
``` python
from multiprocessing import Process
import os
# 子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process will start.')
    p.start()
    p.join()
    print('Child process end.')
```
### 进程池 pool
对Pool对象调用join()方法会等待所有子进程执行完毕，调用join()之前必须先调用close()，调用close()之后就不能继续添加新的Process了
p = Pool(5)
就可以同时跑5个进程。 pool的默认大小是cpu的核数

### 子进程
subprocess 启动并控制子进程
``` python
import subprocess

print('$ nslookup www.python.org')
r = subprocess.call(['nslookup', 'www.python.org'])
print('Exit code:', r)
```
### 进程间通信
multiprocessing模块包装了底层的机制，提供了Queue、Pipes等多种方式来交换数据。

## 多线程
python提供两个模块：_thread(低级模块)和threading(高级，常用)
启动线程就是创建thread实例，调用start()开始执行
启动一个线程就是把一个函数传入并创建Thread实例，调用start()开始执行
``` python 
def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)

print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)
```

### Lock
在多进程中，同一个变量，各有一份拷贝存在于每个进程中，互不影响，

在多线程中个，所有的变量都是线程共享，所以每个变量都能被任何一个线程修改，也是危险所在。

当多个线程调用同一个变量时，可能出现变量值出现修改冲突，影响最终结果。因此在调用时可以采取threading.Lock()来实现。
``` python
balance = 0
lock = threading.Lock()

def run_thread(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()
```
当多个线程同事执行lock.acquire()时，只有一个线程能成功获取锁，在它执行完之后下一个线程才能执行。
但会使代码的程序的效率降低。

### 多核cpu
Python的线程虽然是真正的线程，但解释器执行代码时，有一个GIL锁：Global Interpreter Lock，任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。这个GIL全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。

Python虽然不能利用多线程实现多核任务，但可以通过多进程实现多核任务。多个Python进程有各自独立的GIL锁，互不影响。