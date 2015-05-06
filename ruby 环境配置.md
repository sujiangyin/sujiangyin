1、安装selenium-ide 2.9.0版本
    只需要把未压缩安装包拖入火狐浏览器的附加组件，安装重启。
2、安装selenium-server 2.45.0版本
    只需要在selenium-server目录下输入cmd命令：java -jar selenium-server.jar
    把这条命令打包放在一个jar文件里（要在selenium-server目录下），通过jdk的java.exe关联，创建快捷方式，然后双击就可启动服务器。
3、下载ruby2.2.2版本。点击ruby.exe。ruby -v检测是否安装成功。
4、安装rubygem，下载rubygem.2.4.6，点击setup.rb，gem -v检测安装成功。
5、Ruby 1.9

现在，默认的Ruby 1.9包已经在大多数平台中自动包含RubyGems了 (目前Debian系统将 RubyGems分离到另一个包中)。这就意味着在Ruby 1.9及以上版本，你不需要在加载gem 库前在程序中添加require 'rubygems'
（已经在环境变量中设置）
6、selenium-IDE可产生合理的ruby代码，但却要求使用老版本的gem。。。所以要改动一些代码，看参考数84页。
7、在环境变量的path中写ruby的安装路径。
8、http://www.51testing.com/html/09/46209-89169.html。ruby的测试。
