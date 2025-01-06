# 清理空文件夹

Python脚本

```python
# 2025-01-06 copilots随便搓的，能用，但没有仔细考虑蛋炒饭之类的问题
import os

def find_empty_folders(target_path):
    empty_dirs = []
    for root, dirs, files in os.walk(target_path):
        if not dirs and not files:
            empty_dirs.append(root)
    return empty_dirs

def clean(txt_path):
    with open(txt_path, "r", encoding="utf-8") as f:
        for line in f:
            folder = line.strip()
            if folder and os.path.isdir(folder):
                try:
                    os.rmdir(folder)
                    print(f"已删除文件夹: {folder}")
                except OSError as e:
                    print(f"删除失败: {folder}, 请确认是否为空文件夹: {e}")

def check():
    path = input("请输入目标路径: ")
    empty_folders = find_empty_folders(path)
    if not empty_folders:
        print("未发现空文件夹。")
        return
    print("发现以下空文件夹:")
    for folder in empty_folders:
        print(folder)
    choice = input("是否删除这些文件夹(y/n)?若拒绝，会将检测到的空文件夹保存到txt文件，供后期批量删除 ")
    if choice.lower() == 'y':
        for folder in empty_folders:
            os.rmdir(folder)
        print("空文件夹已删除。")
    else:
        with open("empty_folders.txt", "w", encoding="utf-8") as f:
            for folder in empty_folders:
                f.write(folder + "\n")
        print("已将空文件夹列表保存到 empty_folders.txt")

while True:
    print("1. 根据txt文件清理空文件夹\n2. 检测空文件夹并进行后续处理\n3. 退出")
    choice = input("请选择操作: ")
    if choice == "1":
        txt_path = input("请输入包含文件夹路径的txt文件路径: ")
        clean(txt_path)
    elif choice == "2":
        check()
    elif choice == "3":
        break
    else:
        print("无效输入，请重新输入")
```