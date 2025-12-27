## openpyxl

https://pypi.org/project/openpyxl/

https://openpyxl.readthedocs.io/en/stable/api/openpyxl.workbook.workbook.html#openpyxl.workbook.workbook.Workbook

```python
from openpyxl import Workbook
wb = 工作簿()

# 获取当前工作表
ws = wb.active

# 数据可以直接分配给单元格
ws['A1'] = 42

# 也可以向后追加行
ws.append([1, 2, 3])

# Python 类型将自动转换
导入日期时间
ws['A2'] = datetime.datetime.now()

# 保存文件
wb.save("sample.xlsx")
```

