# shell programming

---

$ read salutation

 Wie geht’s? 

$ echo $salutation 

Wie geht’s?

通常，脚本中的参数由空格字符（例如，空格、制表符或换行符）分隔开。如果希望参数包含一个或多个空白字符，则必须引用该参数。

变量（如$foo inside quotes）的行为取决于您使用的引号类型。如果将$variable表达式括在双引号中，则在执行该行时，它将被替换为其值。如果用单引号括起来，则不会发生替换。您还可以通过在$符号前面加一个\，来删除它的特殊含义。

通常，字符串用双引号括起来，这样可以防止变量被空格分隔，但允许进行$扩展。

![1596670993249](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596670993249.png)

if test -f fred.c

 then ... fi

![1596671503077](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1596671503077.png)

```
if [ “$timeofday” = “yes” ]/注意变量为字符串的时候尽量加引号
```

for循环

```
for variable in values 
do
statements
done
exit 0
```

case语句

```
case variable in 
pattern [ | pattern] ...) statements;; 
pattern [ | pattern] ...) statements;;
... 
esac
```

