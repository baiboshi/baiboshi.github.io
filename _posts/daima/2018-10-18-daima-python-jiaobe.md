---
layout: post
title: 'pyhon批量处理文件'
tags: [daima]
---
```python
##python处理文件脚本
# -*- conding: UTF-8 -*-
master=input("请输入你要处理的数据的名字：")
fh = open(master+".TextGrid")      #打开输入的文件名不包含后缀
for line in fh.readlines():         #读取文件每行内容
    newStr = line.strip()            #过滤掉两边的空格
    if newStr.startswith('text') == True: #只打印包含text字段的行
        mkdir = 'text = "silent"' #定义一个变量
        if newStr != mkdir:   #if 输出非text = "silent" 的行
            mkdir1 = 'text = "sounding"' #定义一个变量
            if newStr != mkdir1:    #if 输出非text = "sounding" 的行
                mkdir2 = 'text = "error"' #定义一个变量
                if newStr != mkdir2: #if 输出非text = "error" 的行
                    print (newStr)
                else:
                    continue #error的行跳过
            else:
                continue   #sounding行跳过
        else:
            continue    #silent行跳过
    else:
        continue      #非包含text的行跳过
```

##过滤硬盘下所有后缀是wav格式的音频文件
```python
import os
import shutil
dtest=os.path.normpath('F:\要处理的数据')
def all_path(dirname):
    #filelistlog = dirname + "\\filelistlog.txt"  # 保存文件路径
    postfix = set(['wav','WAV'])  # 设置要保存的文件格式
    for maindir, subdir, file_name_list in os.walk(dirname):
        for filename in file_name_list:
            apath = os.path.join(maindir, filename)
            #if True:  # 保存全部文件名。若要保留指定文件格式的文件名则注释该句
            if apath.split('.')[-1] in postfix:# 匹配后缀，只保存所选的文件格式。若要保存全部文件，则注释该句
                print (apath)
                #shutil.copyfile(apath,'F:\\ddd')
                #shutil.copy(apath,dtest )
                try:
                    with open(filelistlog, 'a+') as fo:
                        fo.writelines(apath)
                        fo.write('\n')
                except:
                    pass  # 所以异常全部忽略即可


if __name__ == '__main__':
    dirpath = "F:"  # 指定根目录
    all_path(dirpath)
```

