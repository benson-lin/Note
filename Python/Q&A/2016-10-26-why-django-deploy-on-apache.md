# Django自己就可以运行，为什么要部署到apache?

原链接地址：https://www.zhihu.com/question/35540397

# 答一

django自带的web server目的是方便开发，不是能直接放到生产环境的
直接引用django的文档
It’s intended only for use while developing. (We’re in the business of making Web frameworks, not Web servers.)
你可以用ApacheBench tool (ab) 测一下看看django本身的web server能支持多少请求，在把同样的django部署到apache上看看能支持多少请求

# 答二

1. 自带的跑起来首先只有一个实例，性能捉鸡。
2. 自带server只有在debug模式下可用映射静态文件，而debug模式下运行会不断留存debug信息，跑久了内存要爆。
3. 作为服务启动，一个错误就可以挂掉整个服务，起个apache或者eginx好歹挂了只挂个wsgi线程。

# 答三

Django内置http服务器能工作单线程做发调试候
产环境通都用户并发所要用apache做前端另外静态文件处理要由apache做djangosimple HTTP server处理量静态文件性能太差