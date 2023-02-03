# IAR Clean tools

![Language](https://img.shields.io/badge/Language-Python3.8+-blue.svg?style=flat)

[中文说明](#IAR 清理工具)

A simple script is used to clean up the IAR (embedded IDE) installation directory space. It is written in pure python
and has no external dependencies. It supports package migration, compression and deletion for manufacturer chips

**TodoList:**

- Differentiate by chip rather than chip manufacturer
- Restore from backup zip file
- Preference chip support package

## Case

When IAR is installed, all supported chip support packages are installed to the installation directory by default. This
is an out-of-the-box strategy. The advantage is that it can still work quickly when it is used offline or the network is
poor. The obvious disadvantage is that the installation directory is bloated

Take the `IAR EW for Arm 9.30.1` version as an example, which is installed to`C:\Program Files\IAR Systems`by default. A
total of 8.1GB of hard disk space is used, in which the`arm\config`directory contains the support package, which
occupies 4.3GB. Among these chip support packages, many chips are not commonly used or even difficult to purchase, so we
want to delete this part to free up disk space

At the same time, the chip model selection interface inside the IAR software will also be difficult to find the required
chip model because too many chip support packages are installed

The top space occupied in`arm\config\`:

- NXP: 1.9GB
- TexasInstruments: 633.2MB
- Microchip: 388.6MB
- Infineon: 353.1MB
- ST: 282.9MB

## usage

### Simple

Copy the script to the IAR software installation directory (the same level as the `arm\` directory). Use python3 to
execute the script

### Manual

Use python to execute the script through the terminal and add parameters

```powershell
sudo python clean.py "C:\Program Files\IAR Systems\Embedded Workbench 9.1" D:\backup
```

- Parameter 1 is used to specify the installation directory of IAR
- Parameter 2 is used to specify the backup directory. The default directory is the current user document directory.
  This directory needs to exist

### Commands

The commands`move`and`del `require administrator privileges because they require the operating system root directory

#### ? or help [command]

Output help information

#### clear

Built-in clear screen command

#### list

List the installed chip support packages (manufacturer name) in the IAR installation directory

#### size

List the size of the chip support package subdirectory in the IAR installation directory and the free space of the user
backup directory

#### move [manufacturer]

Migrate the chip support package in the IAR installation directory to the user backup directory

#### zip [manufacturer]

Compress the chip support package in the IAR installation directory to the `backup` directory of the user backup
directory

#### del [manufacturer]

Remove the chip support package from the IAR installation directory

#### exit

just exit the tool

## Example

Run the script with administrator privileges and specify the installation directory and backup directory:

```powershell
sudo python clean.py "C:\Program Files\IAR Systems\Embedded Workbench 9.1" D:\backup
```

Enter the script built-in command line:

```powershell
# list current all packages
list
# backup ST packages zip file
zip ST
# or 
zip st
# input 'texas' automatically completes to 'TexasInstruments'
zip texas
# delete TexasInstruments packages
del t
```

In the above command, the manufacturer name currently existing in`arm\config\devices\` will be displayed first.
The `zip`, `del`, and `move` commands support case and automatic completion. When the input chars matches the
corresponding manufacturer name, it will be automatically completed to the full name. When the input chars has multiple
matching names, such as` del t `command above, a list will pop up waiting for the user to enter and select one.

----

## Other

It is recommended to use zip to backup the chip support package before deleting that, unless you are very sure that you
will not use any products of this manufacturer

Compression is a good way to save. For example, the NXP chip support package originally had 1.9GB, but after
compression, it was only 84.6MB. In fact, the entire`arm\config`package was only 200MB+

In fact, you can continue to reduce the size by deleting other infrequent/optional folders, but it is unnecessary and
unsafe

At present, the backup can only be restored by manually copying the contents of the compressed package to the IAR
installation directory

It is recommended to make your own chip support package according to your common use. Extract the common chip
configuration file from the chip package of each manufacturer to synthesize your own chip support package

# IAR 清理工具

![Language](https://img.shields.io/badge/Language-Python3.8+-blue.svg?style=flat)

[中文说明](#IAR 清理工具)

简单的脚本用于清理IAR(嵌入式芯片IDE)安装目录空间.使用纯python编写,没有外部依赖.支持对芯片支持包迁移,压缩,删除.

**TodoList:**

- 按照芯片区分而不是芯片厂商
- 从备份恢复
- 偏好芯片支持包

## 起因

IAR安装的时候默认安装全部支持的芯片支持包到安装目录.这是一种开箱即用的策略,好处是当离线使用或网络不佳时仍可快速进行工作,缺点也显而易见的导致安装目录臃肿.

以`IAR EW for Arm 9.30.1`版本为例,默认安装至`C:\Program Files\IAR Systems`.总共使用8.1GB硬盘空间,其中`arm\config`
目录保存着芯片支持包占用了4.3GB.在这些芯片支持包中,很多芯片都是不常用的甚至很难采购到的,因此我们想要删除这部分以释放磁盘空间.

同时,在IAR软件内部的芯片型号选择界面也会因为安装了过多的芯片支持包而很难找到需要的选择芯片型号.

`arm\config\`中占用空间排名前几名:

- NXP: 1.9GB
- TexasInstruments: 633.2MB
- Microchip: 388.6MB
- Infineon: 353.1MB
- ST: 282.9MB

## 用法

### 简易

将脚本复制到IAR软件安装目录(与`arm\`目录同级目录).使用python执行脚本.

### 手动

通过终端使用python执行脚本,可传递参数

```powershell
sudo python clean.py "C:\Program Files\IAR Systems\Embedded Workbench 9.1" D:\backup
```

- 参数1用来指定IAR的安装目录
- 参数2用来指定备份目录.默认目录为当前用户文档目录.需要此目录已经存在.

### 命令

命令`move`,`del`由于需要操作系统根目录,因此需要获取管理员权限.

#### ? or help [command]

输出帮助信息

#### clear

内置清屏命令

#### list

列出IAR安装目录内已安装的芯片支持包(厂商名称)

#### size

列出IAR安装目录中芯片支持包子目录尺寸和用户备份目录剩余空间

#### move [manufacturer]

迁移IAR安装目录中芯片支持包至用户备份目录

#### zip [manufacturer]

压缩IAR安装目录中芯片支持包至用户备份目录的`backup`目录

#### del [manufacturer]

删除IAR安装目录中芯片支持包

#### exit

退出工具

## 示例

使用管理员权限运行脚本并指定安装目录和备份目录:

```powershell
sudo python clean.py "C:\Program Files\IAR Systems\Embedded Workbench 9.1" D:\backup
```

进入脚本内置命令行:

```powershell
# list current all packages
list
# backup ST packages zip file
zip ST
# or 
zip st
# input 'texas' automatically completes to 'TexasInstruments'
zip texas
# delete TexasInstruments packages
del t
```

上面的命令中,首先会展示当前存在`arm\config\devices\`中的厂商名称,其中`zip`,`del`,`move`
命令支持大小写和自动补全.当输入值匹配到对应厂商名称时将会自动补全至全名.当输入值存在多个匹配值,如上面的`del t`
时,会弹出一个列表等候用户输入选择.

----

## 其他

推荐使用zip的方式先将芯片支持包备份后在删除芯片支持包,除非你十分确认你不会用到这个芯片厂商的任何产品.

压缩是很好的保存手段,比如NXP的芯片支持包原本有1.9GB,压缩后仅有84.6MB.实际上整个`arm\config`的压缩包也只有200MB+.

实际上还能后通过删除其他的不常用的/可选的文件夹以继续缩减尺寸,但没必要还不安全.

目前只能通过手动将备份的压缩包内容复制到IAR安装目录来实现恢复.

推荐按照个人常用制作属于你个人的芯片支持包.将常用的芯片配置文件从各厂商的芯片包中提取出来合成个人专属的芯片支持包.

