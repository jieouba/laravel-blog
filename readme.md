# 博客项目实战构建---环境配置


###  	1. 首先安装composer
###  	2. 下载安装laravel
	Cmd到bolg文件地址输入下面命令：
	Composer create-project laravel/laravel --prefer-dist
###  3. 将下载好的laravel项目打开，首先将server.php该问index，然后将public文件中的httpcss伪静态类移动到blog文件夹，也就是public外面，然后访问配置的本地域名，查看是否成功。
###  	4. 然后进行修改数据库的前缀：
	在config（配置）文件夹中找到database.php文件，在prefix（前缀）后面添加下面代码；
	'prefix' => env('DB_PREFIX', ''),
###  	5. 打开env文件，给数据库添加前缀：
	DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=blog1
    DB_PREFIX=blog_
###  	6. 配置路由，测试数据库是否连接成功：
	首先用命令创建一个控制器，输入下面命令：php artisan make:controller indexController

	然后会在controller文件夹中找到刚刚创建的控制器，
	首先输入DB::引入必要的命名空间，然后删除DB::,查看是否引用DB类，若没有则需要手动输入引用：
    然后在控制器中新建一个方法，

###  	7. 配置路由，测试数据库是否连接成功：
	打开route.php文件，进行配置路由：

###  	8. 然后去访问，会出现以下错误：

	说是没有找到数据库，这时候我们需要在新建一个数据库，用于存放数据。

	打开localhost/phpmyAdmin点击新建数据库，然后选择排序规则为utf-8 unicode

	然后重新打开ecv文件，进行配置，数据库名，以及本地数据库的名称以及密码：


###  	9. 重新加载打开域名查看结果，会出现数据库中的信息，就表明数据库连接成功

###  	10. 到此我们首先将blog环境配置成功。


### 博客项目实战构建--后台超级管理员密码修改及Validation验证
后台超级管理员密码修改及Validation验证

2017年7月6日
22:10

	1. 首先创建一个路由，作为修改密码的规则：

	2. 在indexController控制器中进行创建pass方法，用于返回修改密码的视窗，然后进行将pass.html改为模板类型，将pass模板进行修改，首先进行引入我们以前创建的子视图，

	修改模板，引入子视图；

	3. 进行访问pass路由，发现我们的模板已经加载成功，下一步进行做表单提交，首先找到模板的token值，更改他的token值，

	以及将form的提交方式更改，更改如下：

	4. 然后进行数据的验证，从我们的修改密码的界面可以看出，我们首先是需要输入正确的原密码，然后通过判断用户input中的密码是否与解密后的密码相同，若相同，则进行，下一步，输入新密码，若不对，则提示用户输入的原密码错误，验证原密码成功后，然后用户进行输入新密码，若新密码小于6位或者大于20位，则返回提示信息输入的新密码必须在6-20位之间！，若两次输入的密码不一致，提示用户重新输入，有了这样的思路，我们进行下一步代码的编写：

	5. 首先我们需要读取input中的内容，并进行判断，若有数据则进行数据处理，若没有数据，则重新返回到修改密码的界面。



	6. 然后我们需要引入引入validator服务，make的第一个为需要验证的参数（提交过来的表单），第二个为验证规则（得我们自己定义规则，是一个数组），第三个参数为提示信息，把验证的信息用变量$validator来接收，然后我们进行判断是否验证成功。

	我们可以通过使用$validator中的第三个参数，（第三个参数为提示信息），来获取验证的错误信息，他的参数为一个数组，所以我们需要定义一个数组，数组中，对那个字段验证就输入字段，.后面为验证的规则，=>后面为提示的信息。补全所有的规则以及返回的信息。

	7. 我们需要判断新密码与确认密码相同，在验证条件中，输入confrime来检查是否匹配。给他一个匹配条件，然后我们需要说明用哪个字段来进行匹配，我们只能使用匹配条件中的默认的规则，我们自己定义的input中的再次确认密码输入框，name为password_c，confrime这个方法是不识别的，所以，我们需要将输入面的input标签的name改为password_confrimation，这样就可以知道与哪个字段匹配。



	然后我们将匹配的提示信息进行修改，提示用户两次输入的不一样。

	8. 然后若用户输入的不符合我们的规则，则需要将错误信息传回到我们的页面，这里使用withErrors函数，这个函数中有一个参数，就是我们验证的参数， reture back就是若填写错误，就返回到修改密码的窗口，也就是刷新一个这个修改密码的页面。我们也必须要在页面中要打印出我们返回的错误信息，然后我们将提示信息放在我们修改密码的界面中，打开修改密码的模板，找到那个显示提示信息的div，

	首先，我们需要用if语句去判断是否有错误信息，他传回的是一个数组，判断是否大于零。若小于零，就是用户没有填错，若大于零，则打印用户填写错误的对应的提示信息。我们就使用foreach读取errors数组中所有的错误信息 ，

	9. 然后我们进行判断用户的输入的原始密码是否与数据库中的密码相同，首先从数据库中读取用户密码信息，然后通过对密码进行解析。判断用户输入的密码与数据库中的是否有相同，User：first（），用Crypt:decrypt进行密码解析，然后用if进行判断。若输入的原面正确，则我们进行修改它的密码，用表单提交的用户输入的新密码来作为用户的密码，
	 $user->user_pass=Crypt::encrypt($input['password']);
	然后进行数据保存:$user->update();
	然后修改完成后，就返回到info信息界面。
	return redirect('admin/info');

	10. 然后我们进行最后一步，在index模板中，点击上面的修改密码，就会跳转到修改密码的界面。

	然后我们打开index模板，在a标签中通过url将连接跳转到我们的修改密码界面。

