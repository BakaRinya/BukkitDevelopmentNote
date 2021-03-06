# 事件与事件的监听
  
# 事件
## 事件的概念
事件用来描述服务器内的一切变化与行为. 所有的事件都可以被监听.    
简而言之, 事件就是服务器里发生的事.  
例如, 天气的变化, 玩家的移动. 玩家把树打掉, 又捡起了掉落地上的原木. 这些都是事件.  

这也就意味着, 利用BukkitAPI, 我可以监听天气的变化, 在天气改变时执行某些代码.   

事件分为可控事件和不可控事件. 其最大区别在于能不能取消(*也就是能不能setCancelled*).   
不难理解, 玩家如果退出服务器, 这不能被取消, 它是不可控事件. 玩家的移动可以被取消, 它是可控事件.   

## 事件的用途
> 想象自己正在做一款登录插件, 登录插件是怎么制作出来的呢?  

如果你正在写一个登录插件, 那么你需要监听玩家登录服务器的事件, 玩家进入服务器以后, 记录存储起来他的用户名. 等待玩家输入指令进行登录, 登录完毕以后去掉他的用户名.  
然后再监听其他的各种事件, 判断玩家用户名有没有存储起来, 如果有, 那么他没有登录, 从而取消其他事件.

通过这样的例子可以发现, 事件是一个插件最重要的组成部分!

# 监听器  
我们往往会想在一个事件被触发时, 执行一些代码, 此时我们便需要监听器.  

监听器本质上是一个实现`Listener`类的类. 其中有一些带有`@EventHandler`注解的方法.  
当某个事件触发时, Bukkit将会对应地调用这些带`@EventHandler`方法.   
在BukkitAPI中, 每一个事件都对应一个类. 事件类一般以Event结尾, 所以它们都叫做XXXXXEvent. 所有的事件都在org.bukkit.event包内. Bukkit中所有的事件均可在JavaDoc中查阅.   

下面通过一个示例，了解如何监听一个事件:  

```java
//这里为了偷懒, 直接把主类实现Listener作为监听器
//小的插件可以这样做, 节省开发时间成本(偷懒)
//如果是大插件, 建议采用之后我们将介绍的方法, 让代码更有条理, 方便开发
public class HelloWorld extends JavaPlugin implements Listener{
    public void onEnable(){  
        this.getLogger().info("Hello World!");  
        Bukkit.getPluginManager().registerEvents(this,this); //这里HelloWorld类是监听器, 将当前HelloWorld对象注册监听器  
    }  
  
    public void onDisable(){}  
  
    @EventHandler //这个注解告诉Bukkit这个方法正在监听某个事件, 玩家移动时Bukkit就会调用这个方法
    public void onPlayerMove(PlayerQuitEvent e){   
        System.out.println("玩家退出了！");  
    }  
}
```
`PlayerQuitEvent` 事件可以监听玩家退出服务器, 这是一个不可控事件. 这可以实现监听到玩家退出服务器, 并且在后台输出`玩家退出了！`的字符串.   

在`onEnable`方法中, `registerEvents`方法注册了该监听器.   
*registerEvents方法的第一个参数是监听器，第二个参数是插件主类的实例. 在这里主类就是监听器. 具体你可以在后面了解到*.    

监听器中带有`@EventHandler`的方法一个只能监听某一个事件, 而不能监听多个事件!

# 可取消事件与不可取消事件
> 事件分为可取消事件和不可取消事件. 其最大区别在于能不能取消(*也就是能不能setCancelled*).   

有一个事件是`PlayerMoveEvent`, 顾名思义, 它将会在玩家移动时触发.  
在JavaDoc中, 我们可以与该事件有关的信息. 关于玩家移动的JavaDoc文档可以在下面的链接中看到.  
https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerMoveEvent.html  

> JavaDoc是每个Bukkit插件开发者都需要查阅的资料, 官方版本地址为:
> https://hub.spigotmc.org/javadocs/spigot
>    
> 打开后, 你可以翻阅各个包、各个类, 查看各个方法的具体使用方式.  

在文档中, 我们可以注意到这些内容:  
```java
public class PlayerMoveEvent
extends PlayerEvent
implements Cancellable
```
`PlayerMoveEvent`事件实现了`Cancellable`接口.  
`Cancellable`中定义了`setCancelled`方法和`isCancelled`方法.  
通过`setCancelled`方法, 你可以在事件触发时设置是否取消该事件. 例如, 如果监听玩家移动, 事件触发时使用`setCancelled`方法, 可以取消玩家移动.  
`isCancelled`方法可以判断该事件是否被取消.

对于不可取消事件, 它们没有实现`Cancellable`接口, 因此它们无法被取消.  
就像玩家退出服务器, 你不能像刀剑神域一样, 不让玩家退出服务器.  

下面是一个取消所有玩家在服务器内移动的例子.  
```java
package tdiant.helloworld;  
  
import org.bukkit.plugin.java.JavaPlugin;  
import org.bukkit.event.Listener;
import org.bukkit.event.EventHandler;
  
public class HelloWorld extends JavaPlugin implements Listener {  
    // 关于onEnable与onDisable方法的作用会在下一节具体介绍
    @Override  
    public void onEnable(){  
        //your code here.  
    }  
  
    @Override      
    public void onDisable(){  
        //your code here.  
    }
	
	@EventHandler
	public void onPlayerMove(PlayerMoveEvent e){
	    e.setCancelled(true); //这样就可以设置玩家都移动不了了.
		e.setCancelled(false); //设置了以后还可以修改, 例如这样玩家又可以继续移动了.
		e.setCancelled(true); //现在又改成不取消了
	}
}  
```

值得注意的是, 如果玩家并没有改变他的X/Y/Z, 而只是利用鼠标转了一下身, 这也属于玩家移动, 仍会触发`PlayerMoveEvent`事件.

# 理解客户端与服务端的关系
如果你实际去使用上面的那个代码, 你可能会发现一个问题: 玩家移动在游戏里还可以移动, 但是一会儿会被服务器"弹回来".  
这样确实是达到了取消玩家移动的目的, 但是, 为什么最终的效果不是"玩家一点都动不了"呢?

事实上, 我们无法在服务端取消玩家一点也不能移动.  
客户端移动玩家时, 会在客户端显示出移动后的样子, 然后才会传递给服务器玩家移动的信号, 服务端收到客户端的信号后, 服务器才会触发`PlayerMoveEvent`事件, 做出响应.  

也就是说, 客户端与服务端之间, 客户端往往都是"先斩后奏"的. 客户端不管你服务端取不取消, 先那么显示出来再说.  

*如果要是真的想实现让玩家在服务器的某个坐标一点也动不了, 也许需要发挥你的聪明才智了. 让玩家卡在一个透明方块里? 也许有更好的方案? 现在有人已经实现了!*  
*目前我们通常利用设置玩家移动速度的方法来让玩家无法移动!*  

# EventHandler注解的参数
##监听优先级
想象一下, 如果有两个插件, 他们同时监听玩家移动. 其中一个插件判断后发现玩家没有充够450块钱, 于是它取消了这名玩家的移动. 但是另外一个插件判断后发现玩家非常帅, 于是它允许了这名玩家的移动.  
那么就会存在问题: 有一个插件`setCancelled(true)`, 而又有插件`setCancelled(false)`. 应该以谁为准?  
那就要看监听优先级了!

下面是两个插件处理`PlayerMoveEvent`的部分:
A插件:
```java
    // A插件
	@EventHandler(priority=EventPriority.LOWEST)
	public void onPlayerMove(PlayerMoveEvent e){
	    System.out.println("testA");
	    e.setCancelled(true);
	}
```
B插件:
```java
    // B插件
	@EventHandler(priority=EventPriority.HIGHEST)
	public void onPlayerMove(PlayerMoveEvent e){
	    System.out.println("testB");
	    e.setCancelled(false);
	}
```
在实际的运行中, 当玩家移动时你会发现, 控制台中先输出了`testA`后输出了`testB`, 玩家都在服务器内可以自如移动.  
这意味着A插件第一个响应了玩家移动, 然后B插件才相应的玩家移动.  
`@EventHandler`注解有一个成员叫做`priority`, 给他设置对应的`EventPriority`, 即可设置监听优先级. 在上面的例子中, Bukkit会在所有的LOWEST级监听被调用完毕后, 再去调用HIGHEST级监听.  

`EventPriority`提供了五种优先级, 按照被调用顺序,为:  
LOWEST < LOW < NORMAL(如果你不设置, 默认就是它) < HIGH < HIGHEST < MONITOR .  
其中, LOWEST最先被调用, 但对事件的影响最小. MONITOR最后被调用, 对事件的影响最大.

## ignoreCancelled
`@EventHandler`注解除了`priority`之外, 还有`ignoreCancelled`. 如果不设置, 它默认为false.  

让我们回到上面的A插件与B插件的例子中. 我们把B插件的`onPlayerMove`改成这样:
```java
    // B插件
	@EventHandler(priority=EventPriority.HIGHEST, ignoreCancelled = true)
	public void onPlayerMove(PlayerMoveEvent e){
	    System.out.println("testB");
	    e.setCancelled(false);
	}
```
可以发现, 后台只输出了`testA`, 玩家无法在服务器中移动. 这说明B插件的`onPlayerMove`没有被触发.  
如果有其他监听已经取消了该事件, 设置`ignoreCancelled`为`true`将可以忽略掉这个事件, 所以B插件的`onPlayerMove`方法没有被触发.

## 监听器的注册
可能你已经发现了, 在之前的代码中, 我们都会在`onEnable`方法中插入这样的语句:  
```java
Bukkit.getPluginManager().registerEvents(this,this);  
```
当时解释的是, `registerEvents`方法注册了该监听器.  
如果没有这样的注册语句, 那么Bukkit就不会在事件触发时调用监听器类的对应方法.  

该方法的第一个参数是监听器, 第二个参数是插件主类的实例. 当时由于我们为了偷懒, 直接把主类实现了`Listener`作为监听器, 因此我们可以这样写.    
可我们不能写插件的时候把代码都堆在主类中. 这也就意味着, 我们可以把其他类实现`Listener`, 用同样的方式注册它, 这样我们就可以把监听事件部分的代码放在别的地方, 使插件代码更有条理性.     

我们新创建一个类, 让它实现`Listener`, 再写对应的方法监听玩家移动, 就像这样:  
```java
public class DemoListener {
    @EventHandler
	public void onPlayerMove(PlayerMoveEvent e){
	    System.out.println("PLAYER MOVE!");
	}
}
```
现在我们在主类的`onEnable`方法里, 就可以注册它了!  
```java
Bukkit.getPluginManager().registerEvents(new DemoListener(), this);  
```

# 常用事件简介
## 登录、进入服务器
BukkitAPI中与登录有关的常见的有: `PlayerLoginEvent` `PlayerJoinEvent`.  
值得注意的是, 所有玩家进入服务器的事件都是不可取消事件.  

在玩家尝试连接服务器时, 会触发`PlayerLoginEvent`, 玩家完全地进入服务器后, 会触发`PlayerJoinEvent`.  
在`PlayerLoginEvent`触发的时候, 你不可以操控玩家`Player`对象获取其背包等信息, 而仅可以获取UUID、玩家名和网络信息(IP等)等.  
*顺便一提, 玩家如果不在线, 你不可以通过BukkitAPI操控其背包.  *
`PlayerJoinEvent`触发时, 服务器内将会出现玩家实体. 此时你可以当做玩家完全进入服务器, 对其自由操作.

打个比方, 你家有一扇防盗门, 有人想进入你家.  
首先他需要敲门, 在门外喊出自己的基本信息(名字等), 这是`PlayerLoginEvent`触发的时候. 如果你想从他背包里拿出东西, 不可以, 因为他在门外面.  
当你给他打开门, 他进了你家中站稳了以后, 这是`PlayerJoinEvent`触发的时候, 这时候不管你是想打他还是想拿走他的东西, 都可以.
