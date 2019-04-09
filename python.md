# python
> - 获取某目录下的路径，子路径，非目录子文件
>> import os
  def file_name_walk(file_dir):
      fileList = []
      for root, dirs, files in os.walk(file_dir):
          print("root", root)   '''当前目录路径'''
          print("dirs", dirs) '''当前路径下所有子目录'''
          fileList.append(files)
          print("files", files)   '''当前路径下所有非目录子文件'''
      print(fileList)
  file_name_walk("../supply_chain/data/data3/")
