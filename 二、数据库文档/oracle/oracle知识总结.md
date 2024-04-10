# oracle知识总结

## 一、基本函数的总结

1.row_number() 的使用

2.sign()  和decode的用法

3.



### 1.字符函数

| ASCII(x)                 | 返回字符x的ASCII码。                                         |
| ------------------------ | ------------------------------------------------------------ |
| CONCAT(x,y)              | 连接字符串x和y。                                             |
| INSTR(x, str. Start, n)  | 在x中查找str，可以指定从start开始，也可以指定从第n次开始。   |
| LENGTH(x)                | 返回x的长度。                                                |
| LOWER(x)                 | x转换为小写。                                                |
| UPPER(x)                 | x转换为大写。                                                |
| LTRIM(x,trim_str)        | 把x的左边截去trim_str字符串，缺省截去空格。                  |
| RTRIM(x,trim_str)        | 把x的右边截去trim_str字符串，缺省截去空格。                  |
| TRIM(trim_str FROM x)    | 把x的两边截去trim_str字符串，缺省截去空格。                  |
| REPLACE(x,old,new)       | 在x中查找old，并替换为new。                                  |
| SUBSTR(x, start ,length) | 返回x的字串，从staart处开始，截取length个字符，缺省length，默认到结尾。 |



### 2.数字函数

| **函数**   | **说明**              | **示例**            |
| ---------- | --------------------- | ------------------- |
| ABS(x)     | x绝对值               | ABS(-3)=3           |
| ACOS(x)    | x的反余弦             | ACOS(1)=0           |
| COS(x)     | 余弦                  | COS(1)=1.57079633   |
| CEIL(x)    | 大于或等于x的最小整数 | CEIL(5.4)=6         |
| FLOOR(x)   | 小于或等于x的最大整数 | FLOOR(5.8)=5        |
| LOG(x,y)   | x为底y的对数          | LOG(2,4)=2          |
| MOD(x,y)   | x除以y的余数          | MOD(8,3)=2          |
| POWER(x,y) | x的y次幂              | POWER(2,3)=8        |
| ROUND(x,y) | x在第y小数位四舍五入  | ROUND(3.456,2)=3.46 |
| SQRT(x)    | x的平方根             | SQRT(4)=2           |
| TRUNC(x,y) | x在第y位截断          | TRUNC(3.456,2)=3.45 |



### 3.日期函数

| 函数                   | 说明                      | 示例                      |
| ---------------------- | ------------------------- | ------------------------- |
| sysdate                | 获取系统当前的时间        | select sysdate from dual; |
| to_char(sysdate,'Q')   | 当前日期所在季度          |                           |
| to_char(sysdate,'mm')  | 当前日期在本年的第几个月  |                           |
| to_char(sysdate,'ww')  | 当前日期是‘本年’的第几周  |                           |
| to_char(sysdate,'w')   | -当前日期是‘本月’的第几周 |                           |
| to_char(sysdate,'ddd') | 当前日期是本年第几天      |                           |
| to_char(sysdate,'dd')  | 当前日期是本月第几天      |                           |
|                        |                           |                           |

