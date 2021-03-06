ss: 获得socket统计的另一种方法
============================================================

在之前的文章中,我有提到**ss**, _iproute2_ 包附带的另一个工具, 允许你查询socket的有关统计信息. 可以完成**netstat**同样的任务, 但是,ss稍微快一点而且命令更简短.

直接输入**ss**, 默认会显示与netstat同样的内容, 并且输入类似的参数可以获取你想要的类似输出. 例如:

```
$ ss -t
State       Recv-Q Send-Q       Local Address:Port                        Peer Address:Port
ESTAB       0      0                127.0.0.1:postgresql                     127.0.0.1:48154
ESTAB       0      0            192.168.0.136:35296                      192.168.0.120:8009
ESTAB       0      0            192.168.0.136:47574                     173.194.74.189:https
[…]
```


**ss -t** 只显示TCP连接. **ss -u** 用于显示UDP连接, **-l** 只会显示监听的端口,而且可以进一步过滤到任何想要的信息。

我并没有测试所有可用参数, 但是可以使用 **-K** 强制关闭socket.

**ss**真正耀眼的地方是内置的过滤能力.让我们列出所有端口为22(ssh)的连接:

```
$ ss state all sport = :ssh
Netid State      Recv-Q Send-Q     Local Address:Port                      Peer Address:Port
tcp   LISTEN     0      128                    *:ssh                                  *:*
tcp   ESTAB      0      0          192.168.0.136:ssh                      192.168.0.102:46540
tcp   LISTEN     0      128                   :::ssh                                 :::*
```

如果只想看已建立的socket(排除了 _listening_ 和 _closed_ ):

```
$ ss state connected sport = :ssh
Netid State      Recv-Q Send-Q     Local Address:Port                      Peer Address:Port
tcp   ESTAB      0      0          192.168.0.136:ssh                      192.168.0.102:46540
```

类似的, 可以列出指定的host或者ip段; 例如, 列出到达74.125.0.0/16子网的连接, 这个子网属于Google:

```
$ ss state all dst 74.125.0.0/16
Netid State      Recv-Q Send-Q     Local Address:Port                      Peer Address:Port
tcp   ESTAB      0      0          192.168.0.136:33616                   74.125.142.189:https
tcp   ESTAB      0      0          192.168.0.136:42034                    74.125.70.189:https
tcp   ESTAB      0      0          192.168.0.136:57408                   74.125.202.189:https
```

ss与 _iptables_ 的语法非常相同, 如果已经熟悉了其语法, ss非常容易上手. 也可以安装 _iproute2-doc_ 包, 通过 _/usr/share/doc/iproute2-doc/ss.html_ 获得完整文档.

还不快试试! 你就可以知道它有多棒.无论如何, 让我输入的字符越少我越高兴.

--------------------------------------------------------------------------------

via: https://insights.ubuntu.com/2017/07/25/ss-another-way-to-get-socket-statistics/

作者：[ Mathieu Trudel-Lapierre  ][a]
译者：[VicYu](https://vicyu.com)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:https://insights.ubuntu.com/author/mathieu-trudel-lapierre/
[1]:https://insights.ubuntu.com/author/mathieu-trudel-lapierre/
