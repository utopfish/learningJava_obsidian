首先需要知道，触发JVM进行Full GC的情况：
1、System.gc()方法的调用
2、老年代空间不足
3、永生区空间不足
Permanet Generation中存放的为一些class的信息、常量、静态变量等数据，当系统中要加载的类、反射的类和调用的方法较多时，Permanet Generation可能会被占满，为避免Perm Gen占满造成Full GC现象，可采用的方法为增大Perm Gen空间或转为使用CMS GC。
4、CMS GC时出现promotion failed和concurrent mode failure
5、统计得到的Minor GC晋升到旧生代的平均大小大于老年代的剩余空间
6、堆中分配很大的对象
那么接下来就是去逐步排查具体是什么原因造成的
1. 检查启动参数
1. 一开始就应该通过pstack来看，full gc是什么地方在触发，然后反推
1. 使用命令 jstat -J-Djstat.showUnsupported=true -snap [pid] 查看平均晋升空间
1. jstat -gccause pid 查看gc详细原因
