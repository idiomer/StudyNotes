# python -- re

## 常用

``` python
import re
s = '''imei=123
imei=bax
imei=t4e
imei=935
'''
r = re.compile(r"imei=([\da-z]+)")

########### findall ###########
m = r.findall(s, 0, len(s))
print m
## ['123', 'bax', 't4e', '935']

########### finditer ###########
m = r.finditer(s, 0, len(s))
for x in m:
    # x is SRE_match object
    print "x.group()=%s\tx.group(1, 1)=%s\tx.groups()=%s"%(x.group(), x.group(1, 1), x.groups())
## x.group()=imei=123   x.group(1)=(123, 123)   x.groups()=('123',)
## x.group()=imei=bax   x.group(1)=(bax, bax)   x.groups()=('bax',)
## x.group()=imei=t4e   x.group(1)=(t4e, t4e)   x.groups()=('t4e',)
## x.group()=imei=935   x.group(1)=(935, 935)   x.groups()=('935',)
```



## convention

- "."：匹配除"\n”外的所有字符，除非指定[`DOTALL`](https://docs.python.org/2/library/re.html#re.DOTALL) 标记
- "^"和"$"：的单行模式(默认)和多行模式([`MULTILINE`](https://docs.python.org/2/library/re.html#re.MULTILINE))
- "?"： 禁止"\*", "+", "?"和"{m,n}"的贪婪模式，即"\*?", "+?", "??"和"{m,n}?"开启非贪婪模式
- "\[\]"：set模式
  - -可以当连续符号，如[a-f]表示[abcdef]。但如果在首尾位置或被"\"转义，则表示原意
  - "^", "$", "".", "\*", "+", "?", "(", ")", "{", "}", "|"都是原意。但"^"在首位时，表示反义，如\[^w\]表示除了w之外的所有字符
  - Character classes such as `\w` or `\S` (defined below) are also accepted inside a set, although the characters they match depends on whether [`LOCALE`](https://docs.python.org/2/library/re.html#re.LOCALE) or [`UNICODE`](https://docs.python.org/2/library/re.html#re.UNICODE) mode is in force.
- "reA|reB"：或模式(:短路)
-  "()"



## re

### re.**compile**(*pattern*, *flags=0*)

**flags** can be zore or more **constants** that describes below，用"|"组合在一起。

- re.DEBUG
- re.IGNORECASE (re.I)
- re.LOCALE (re.L): Make `\w`, `\W`, `\b`, `\B`, `\s` and `\S` dependent on the current locale.
- re.MULTILINE (re.M)
- re.DOTALL (re.S)
- re.UNICODE (re.U): Make `\w`, `\W`, `\b`, `\B`, `\d`, `\D`, `\s` and `\S` dependent on the Unicode character properties database.
- re.VERBOSE (re.X): write readable re



### re.**search**(*pattern*, *string*, *flags=0*)

匹配字符串中第一个符合pattern的子串。



### re.**match**(*pattern*, *string*, *flags=0*)

只在字符串开始处(即^处)开始匹配的search。即使在 [`MULTILINE`](https://docs.python.org/2/library/re.html#re.MULTILINE) 模式下,也是只匹配开始处，而不是每行的开始处。



### re.**split**(*pattern*, *string*, *maxsplit=0*, *flags=0*)

符合pattern的子串作为分隔符，返回分割后的列表；如果pattern加了"()"，则分隔符也被返回。此外，空pattern是不分割字符串的。



### re.**findall**(*pattern*, *string*, *flags=0*)

概念上相当于search_all，但是没有search_all函数。顾名思义。

注意：当有"()"时，只返回"()"内匹配的子串，即返回groups。

```python
In [1]: import re

In [2]: re.findall(r'imei=\d+', "imei=123aa&imei=456ff")
Out[2]: ['imei=123', 'imei=456']

In [3]: re.findall(r'imei=(\d+)', "imei=123aa&imei=456ff")
Out[3]: ['123', '456']

In [4]: re.findall(r'imei=(\d+)([a-z]+)', "imei=123aa&imei=456ff")
Out[4]: [('123', 'aa'), ('456', 'ff')]

```



### re.**finditer**(*pattern*, *string*, *flags=0*)

findall的iterator版本



### re.**sub**(*pattern*, *repl*, *string*, *count=0*, *flags=0*)

repl可以是re也可以是function



## Regular Expression Object

class re.RegexObject



### search(string[, pos[, endpos]])

### match(string[, pos[, endpos]]) 

### split(string, maxsplit=0)

### findall(string[, pos[, endpos]])

### finditer(string[, pos[, endpos]])

### sub(repl, string, count=0)



## Match Object

class re.MatchObject

### group([group1, ...])

```python
>>> m = re.match(r"(\w+) (\w+)", "Isaac Newton, physicist")
>>> m.group(0)       # The entire match
'Isaac Newton'
>>> m.group(1)       # The first parenthesized subgroup.
'Isaac'
>>> m.group(2)       # The second parenthesized subgroup.
'Newton'
>>> m.group(1, 2)    # Multiple arguments give us a tuple.
('Isaac', 'Newton')


>>> m = re.match(r"(?P<first_name>\w+) (?P<last_name>\w+)", "Malcolm Reynolds")
>>> m.group('first_name')
'Malcolm'
>>> m.group('last_name')
'Reynolds'
>>> m.group(1)
'Malcolm'
>>> m.group(2)
'Reynolds'
```



### groups([default])

```python
>>> m = re.match(r"(\d+)\.(\d+)", "24.1632")
>>> m.groups()
('24', '1632')
```



### string

匹配前给的字符串

