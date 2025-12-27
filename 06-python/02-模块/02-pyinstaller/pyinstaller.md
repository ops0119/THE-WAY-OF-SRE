# pyinstaller

https://pypi.org/project/pyinstaller/

https://pyinstaller.org/en/v6.17.0/installation.html



使用

https://pyinstaller.org/en/v6.17.0/usage.html

| 参数           | 说明                     |
| -------------- | ------------------------ |
| --icon=app.ico | 设置 exe 图标            |
| --name="MyApp" | 指定生成的 exe 名称      |
| --noconfirm    | 覆盖已有构建目录时不提示 |
| --clean        | 构建前清理缓存           |





```python
pip install PyInstaller
pyinstaller -F --onefile  xxx.py
```

找到py文件所在的dist文件夹，里面有个和py文件一样名字的exe文件