# File and Folder

<details>
<summary>batch rename File / Folder</summary>

```bat
:: rename.bat
@echo off
chcp 65001 >nul 2>&1
cd /d "D:\code\boost"

echo ==============================
echo 批量重命名工具启动中...
echo 当前工作目录：%cd%
echo ==============================
echo.

:: 执行uv run命令
uv run "D:\code\boost\rename.py"

echo.
echo ==============================
echo 程序执行完成！
echo 按任意键退出...
pause >nul
```

```python
# rename.py
import os
import re

def parse_replace_rule(rule_str):
    """
    解析替换规则字符串，格式为 /oldstring/newstring/
    :param rule_str: 替换规则字符串
    :return: (old_str, new_str) 元组
    """
    # 去除首尾的空白字符
    rule_str = rule_str.strip()
    
    # 检查规则格式是否正确
    if not (rule_str.startswith('/') and rule_str.endswith('/')):
        raise ValueError("替换规则格式错误！请使用 /oldstring/newstring/ 格式")
    
    # 分割规则字符串
    parts = rule_str[1:-1].split('/', 1)  # 只分割一次，避免新/旧字符串包含/
    old_str = parts[0] if len(parts) > 0 else ''
    new_str = parts[1] if len(parts) > 1 else ''
    
    return old_str, new_str

def batch_rename(path, old_str, new_str):
    """
    批量重命名指定路径下的文件和文件夹
    :param path: 目标路径
    :param old_str: 需要替换的旧字符串
    :param new_str: 替换后的新字符串
    """
    # 检查路径是否存在
    if not os.path.exists(path):
        print(f"错误：路径 '{path}' 不存在！")
        return
    
    # 存储所有需要重命名的项（先文件后文件夹，避免路径问题）
    rename_items = []
    
    # 递归遍历路径
    for root, dirs, files in os.walk(path, topdown=False):  # topdown=False 从最深层开始遍历
        # 处理文件
        for file in files:
            old_file_path = os.path.join(root, file)
            new_file_name = file.replace(old_str, new_str)
            new_file_path = os.path.join(root, new_file_name)
            if old_file_path != new_file_path:  # 只有文件名发生变化时才加入列表
                rename_items.append((old_file_path, new_file_path))
        
        # 处理文件夹
        for dir_name in dirs:
            old_dir_path = os.path.join(root, dir_name)
            new_dir_name = dir_name.replace(old_str, new_str)
            new_dir_path = os.path.join(root, new_dir_name)
            if old_dir_path != new_dir_path:  # 只有文件夹名发生变化时才加入列表
                rename_items.append((old_dir_path, new_dir_path))
    
    # 执行重命名操作
    if not rename_items:
        print("未找到需要替换的文件/文件夹！")
        return
    
    print(f"\n共找到 {len(rename_items)} 个需要重命名的项：")
    success_count = 0
    fail_count = 0
    
    for old_path, new_path in rename_items:
        try:
            os.rename(old_path, new_path)
            print(f"✅ 成功：{old_path} -> {new_path}")
            success_count += 1
        except Exception as e:
            print(f"❌ 失败：{old_path} -> {new_path} | 错误：{str(e)}")
            fail_count += 1
    
    print(f"\n📊 重命名完成：成功 {success_count} 个，失败 {fail_count} 个")

def main():
    print("===== 批量替换文件/文件夹名特殊字符工具 =====")
    print("使用说明：")
    print("1. 输入要处理的文件夹路径（绝对路径或相对路径）")
    print("2. 输入替换规则，格式为 /oldstring/newstring/")
    print("   - 示例1：/【】/  表示去掉所有【】字符")
    print("   - 示例2：/张三/李四/  表示将张三替换为李四")
    print("   - 示例3：/ /_/  表示将空格替换为下划线")
    print("-" * 50)
    
    # 获取用户输入
    while True:
        target_path = input("请输入目标路径：").strip()
        if target_path:
            break
        print("路径不能为空，请重新输入！")
    
    while True:
        rule_input = input("请输入替换规则（格式：/oldstring/newstring/）：").strip()
        if rule_input:
            try:
                old_str, new_str = parse_replace_rule(rule_input)
                break
            except ValueError as e:
                print(f"规则解析失败：{e}，请重新输入！")
        else:
            print("替换规则不能为空，请重新输入！")
    
    print(f"\n即将执行替换：")
    print(f"目标路径：{target_path}")
    print(f"替换规则：将 '{old_str}' 替换为 '{new_str}'（为空则删除）")
    
    # 确认执行
    confirm = input("\n是否确认执行？(y/n，默认y)：").strip().lower()
    if confirm in ['', 'y', 'yes']:
        batch_rename(target_path, old_str, new_str)
    else:
        print("操作已取消！")

if __name__ == "__main__":
    main()
```

</details>

<details>
<summary>batch delete file</summary>

```bat
@echo off
chcp 65001
setlocal enabledelayedexpansion

:: 设置要删除的文件扩展名
set "fileExtension=pdf"

:: 设置要搜索的根目录
set "directory=D:\geektemp"

:: 递归查找并删除指定类型的文件
for /r "%directory%" %%f in (*.%fileExtension%) do (
    echo 删除文件: %%f
    del /f /q "%%f"
)

set "fileExtension1=m4a"
for /r "%directory%" %%f in (*.%fileExtension1%) do (
    echo 删除文件: %%f
    del /f /q "%%f"
)

echo 完成删除操作
pause
```

</details>

# Audio and Video

<details>
<summary>Merge mp4 and srt</summary>

Prerequisite: install ffmpeg, configure PATH environment variable

```bat
@echo off
chcp 65001 > nul
setlocal enabledelayedexpansion

:: ===================== 配置区（可根据需要修改）=====================
:: 源文件所在文件夹（默认是脚本所在文件夹，如需指定路径请修改，例如：set "source_dir=D:\视频文件"）
set "source_dir=%~dp0"
:: 输出文件夹（默认在源文件夹下新建 new 文件夹，如需修改请调整）
set "output_dir=%source_dir%new"
:: 字幕语言（chi=中文，eng=英文，可根据需要修改）
set "subtitle_lang=chi"
:: ===================================================================

:: 创建输出文件夹（如果不存在）
if not exist "%output_dir%" (
    mkdir "%output_dir%"
    echo 已创建输出文件夹：%output_dir%
)

echo 开始批量合并 MP4 和 SRT 字幕...
echo 源文件夹：%source_dir%
echo 输出文件夹：%output_dir%
echo.

:: 遍历源文件夹下所有 MP4 文件
for %%f in ("%source_dir%*.mp4") do (
    :: 获取不带扩展名的文件名（例如：1-1.STL学习介绍）
    set "filename=%%~nf"
    :: 拼接对应的 SRT 字幕文件路径
    set "srt_file=%source_dir%!filename!.srt"
    
    :: 检查对应的 SRT 文件是否存在
    if exist "!srt_file!" (
        echo 正在处理：!filename!.mp4
        :: 执行 FFmpeg 合并命令
        ffmpeg -i "%%f" -i "!srt_file!" -c:v copy -c:a copy -c:s mov_text ^
        -metadata:s:s:0 language=%subtitle_lang% -y "%output_dir%\!filename!.mp4"
        
        if !errorlevel! equ 0 (
            echo ✅ 成功：!filename!.mp4 合并完成
        ) else (
            echo ❌ 失败：!filename!.mp4 合并出错
        )
    ) else (
        echo ⚠️  跳过：!filename!.mp4 未找到对应的 SRT 字幕文件
    )
    echo.
)

echo.
echo 批量处理完成！所有合并后的文件已保存至：%output_dir%
pause
```

</details>

<details>
<summary>Compress Video</summary>

```bat
@echo off
chcp 65001
setlocal enabledelayedexpansion

:: call是同步调用方式，下面的bat脚本里末尾不要有 pause 语句
call ./xxx.bat

call ./zip.bat

......

pause
```

```bat
@echo off
chcp 65001
setlocal enabledelayedexpansion

:: 设置输入和输出目录
set "input_dir=./django"
set "output_dir=./django-zip"

:: 创建输出目录（如果不存在）
if not exist "%output_dir%" mkdir "%output_dir%"

:: 遍历输入目录中的所有mp4文件
for %%F in ("%input_dir%\*.mp4") do (
    set "filename=%%~nF"
    set "extension=%%~xF"

    :: 处理文件名中的特殊字符（如竖线|）
    set "safe_filename=!filename!"

    :: 使用ffmpeg进行压缩
    echo 正在处理: %%F
    ffmpeg -i "%%F" -c:v libx265 -x265-params crf=18 -c:a copy "%output_dir%\!safe_filename!!extension!"
    echo.
)

:: 遍历输入目录中的所有avi文件
for %%F in ("%input_dir%\*.avi") do (
    set "filename=%%~nF"
    set "extension=%%~xF"

    :: 处理文件名中的特殊字符（如竖线|）
    set "safe_filename=!filename!"

    :: 使用ffmpeg进行压缩
    echo 正在处理: %%F
    ffmpeg -i "%%F" -c:v libx265 -x265-params crf=18 -c:a copy "%output_dir%\!safe_filename!.mp4"
    echo.
)

echo 所有视频处理完成
:: pause
```

crf 值的范围是 0-50，0 为无损压缩，20 以内可以视觉上无损，一般设置为 18 就可以，可以将 100M 的视频压缩到 20M

```bash
ffmpeg -i a.mp4 -c:v libx265 -x265-params crf=18 b.mp4
ffmpeg -i "./web/xxx.avi" -c:v libx265 -x265-params crf=18 -c:a copy "./web-zip/xxx.mp4"
```

</details>
