# 大數據分析基本應用
---
## 字串解析並存入字典結構
![圖片04](./assets/images/turtle04.png)
```python
import datetime

f = open('學生資料表.txt', 'r', encoding="big5")
# print(type(f)) # <class '_io.TextIOWrapper'>
# print(dir(f))
data_string = f.read() # 變成一個很大的字串
# print(repr(a))
f.close()

# 利用跳行轉義字符\n切割字串
lines = data_string.split('\n')
# print(lines)

# 创建字典列表
students = []
for line in lines:
    if line.strip():  # 如果是空行就跳過
        parts = line.split()
        # 处理姓名可能包含空格的情况（虽然本例中没有）
        # 假设格式固定为：姓名 学号 成绩一 成绩二 生日
        name = parts[0]
        student_id = parts[1]
        score1 = int(parts[2])
        score2 = int(parts[3])
        birthday = parts[4]

        student = {
            '姓名': name,
            '學號': student_id,
            '成績一': score1,
            '成績二': score2,
            '生日': birthday
        }
        students.append(student)

# 動態新增平均成績欄位
for student in students:
    student['平均'] = ( student['成績一'] + student['成績二'] ) / 2
    #print(student)

# 動態新增年齡欄位
for student in students:
    # step1. 把生日字串變成日期結構  'strftime'日期變字串, 'strptime'字串變日期
    我的生日 = datetime.datetime.strptime(student['生日'],'%Y/%m/%d')
    # step2. 得到今天日期結構
    今天日期 = datetime.datetime.now()
    # step3. 算年齡
    age = 今天日期.year - 我的生日.year
    # step4. 如果生日還沒過  年齡要減一
    if 今天日期 < datetime.datetime(今天日期.year,我的生日.month,我的生日.day) :
        age -= 1
    student['年齡'] = age

a = sorted(students,key=lambda x:(x['年齡'], x['平均']),reverse=True)
for student in a:
    print(student)
```
