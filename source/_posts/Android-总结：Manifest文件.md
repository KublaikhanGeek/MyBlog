title: Android 总结：Manifest文件
date: 2016-10-26 21:35:13
tags: [Android开发]
categories: Android开发
---
### Android 总结： Manifest文件
***

原文链接：[http://blog.csdn.net/u014136472/article/details/49851387](http://blog.csdn.net/u014136472/article/details/49851387)
#### 一、application 属性

< application /> ：应用的声明。
这个元素包含了子元素，这些子元素**声明了应用的组件**，元素的属性将会影响应用下的所有组件。很多属性为组件设置了默认值，**有些属性设置了全局值并且不能被组件修改**。

```html
<application/>节点必须包括在<manifest/>节点中。

而<application/>节点本身还包括
<activity/>,<activity-alias/>,<service/>,<receiver/>,<provider/>和<uses-library/>这几个节点。
```
            
<!--more-->


```html
    <application 
        android:allowClearUserData = ["true" | "false"]  
        android:allowTaskReparenting = ["true" | "false"]  
        android:backupAgent = "string"  
        android:debuggable = ["true" | "false"]  
        android:description = "string resource"  
        android:enabled = ["true" | "false"]  
        android:hasCode = ["true" | "false"]  
        android:icon = "drawable resource"  
        android:killAfterRestore = ["true" | "false"]  
        android:label = "string resource"  
        android:manageSpaceActivity = "string"  
        android:name = "string"  
        android:permission = "string"  
        android:persistent = ["true" | "false"]  
        android:process = "string"  
        android:restoreAnyVersion = ["true" | "false"]  
        android:taskAffinity = "string"  
        android:theme = "resource or theme" >  
        . . .  
    </application> 
```


1. Android:allowClearUserData
是否给以用户删除用户数据的权限.
如果为true应用管理者就拥有清除数据的权限；false没有。默认为true。

2. android:allowTaskReparenting
应用定义的activities是否可以被从启动的任务转移到和他有相同并且将被带到前台的任务。
true他们可以被转移，如果为false，他们必须和启动他们的任务保持在一起。
默认为false。

3. android:backupAgent
实现应用的备份代理的类名，BackupAgent的子类。
这个属性的名称因该是全限定类名(如，”com.example.project.MyBackupAgent”)。
但是，如果名称的首字母被设置为点号，也可以为类名(如，”.MyBackupAgent”)，
他将被追加到在< manifest />元素中定义的包名后。
没有默认值。

4. android:debuggable
应用是否可以使用debug，甚至运行在用户模式下。
true可以，false不能。默认为false。

5. android:description
用户可读的，比应用标签更长、更多的应用描述。
此值必须是一个引用字符串。不像标签，他不能被设置为硬编码字符串。没有默认值。

6. android:enabled
Android系统是否可以实例化应用的组件。
如果为true可以，如果为false不可以。
如果为true，每个组件的enabled属性决定了此组件。
如果为false，他重写了组件指定值，所有的组件将不还用。
默认为true。

7. android:hasCode
应用是否包含代码。
true表示包含，false表示不包含。
当值为false时，在启动组件是系统不会试着加载应用的任何代码。
默认为true。

8. android:icon
整个应用的图标，还是每个组件的默认图标。
这个属性值**必须**被设置为drawable资源的引用。
没有默认值。

9. android:killAfterRestore
在整型系统重置操作中，当他的设置被重置后，应用是否应该被终止。
单个包的重置操作不会引起应用被关闭。
整个系统的恢复操作仅代表性的发生一次，当电话第一次被设置时。
第三方应用将不会经常使用此属性。
默认值为true，意思是，当整个系统被恢复时，应用运行完他的数据后，将会终止。

10. android:label
一个易读的应用标签，并且还是应用的每个组件的默认标签。
这个标签应该被设置为引用字符串资源，当然他也可以像其他字符串一样在用户接口中指定。
但是为了方便，在应用开发时，可以被设置未定义字符串。

11. android:manageSpaceActivity
一个Activity子类的全限定名称，这个Activity可以被系统启动让用户管理此应用占有的存储空间。
这个Activity也应该用< activity />元素声明。

12. android:name
为这个应用实现的Application子类的全限定名称。
**当应用启动时，这个类将在应用的其他组件之前被实例化。**
这个子类是可选的；大多数应用不需要。
在缺省时，Android使用基本Application类的实例。

13. android:permission
客户为了和应用交互必须设置的许可的名称。
这个属性是一个便利的途径为应用的组件设置许可。
他可以被组件的permission属性重写。

14. android:persistent
应用是否在所有时间下都保持运行。
true是，false不是。
默认为false。
通常情况下应用不应该设置此标识。
持久模式仅仅被几个系统应用指定。

15. android:process
为应用下的组件定一个运行进程名称。
每个组件可以定义自己的进程名称通过设置自己的process属性。
在默认情况下，Android为应用创建一个进程，当应用的第一个组件需要运行时。
所有的组件在同一个进程下运行。这个进程的名称和在< manifest />元素设置的backage属性名相同。
通过设置这个属性在可以在其他应用中共享，你可以协调应用的组件在同一个进程中运行，但是只有两应用也共享用户ID和签订相同的证书。
如果这个属性的名称一个冒号(“:”)开始，一个新的私有的进程将被创建。
如果一个进程的名称以小写字母开头，一个公共的进程将被创建。
一个公共的进程可以被其他应用共享，来减少资源的使用。

16. android:restoreAnyVersion
表明这个应用准备尝试恢复所有的备份数据集合，甚至如果备份数据是比当前安装的应用高的编号存储的。
设置为true将允许备份管理者去尝试恢复当版本不匹配，意思是数据冲突。
要小心使用。默认为false。

17. android:taskAffinity
提供给应用下所有组件的类同名称，除了设置了自己的taskAffinity属性的组件。
默认情况下所有的组件使用相同的affinity。
Affinity的名称和在< manifest />元素中设置的包名相同。

18. android:theme
为应用下的组件定义一个引用自样式资源的主题。
个别的activities可以设置自己的主题，通过设置自己的theme属性。

19. android:allowBackup
它表示是否允许应用程序参与备份。
如果将该属性设置为false,则即使备份整个系统,也不会执行这个应用程序的备份操作。
而整个系统备份能导致所有应用程序数据通过ADB来保存。
该属性必须是一个布尔值，或为true，或为false。
默认值为true。

20. android:largeHeap
应用程序是否使用一个比较大的堆创建。
它是一个布尔值，在没有配置的情况下，它的默认值是false。

#### 二、activity 标签属性

1. android:allowTaskReparenting
 * android:allowTaskReparenting是一个任务调整属性。
 * 它表明当这个任务重新被送到前台时，该应用程序所定义的Activity是否可以从被启动的任务中转移到有相同亲和力的任务中。
 * 为什么在这里还要在提一次呢？因为它与< application />的 android:allowTaskReparenting 属性重叠，因此当为正在配置的Activity提供该属性的时候，它的默认值首先来自< application />节点。如果< application/>节点上没有配置该属性的时候，则false就是它的默认值。
 * 通常，当一个Activity启动的时候，Activity管理服务就会为这个Activity生成一个任务并将此Activity与之相关联。在一个任务中可能存在多个Activity，它按照一定顺序排列在这个任务中，我们可以使用这个属性来强制它重新成为此任务的顶层Activity。在当前的任务不再显示时，也就是说，与此Activity相关联任务不在前台显示的时候，可以使用这个特性来强制Activity转移到与之有相同亲和力的任务（taskAffinity属性定义的任务）中。典型的用法是把一个应用程序的Activity移到另一个应用程序的主任务中。
 * 例如，如果我们收到的一条短信（MMS应用程序）中包含一个电话号码文本，此时可以单击电话号码来启动拨号的快捷界面。但是，这个拨号界面是联系人应用程序的一个Activity，在这个场景下，它可能成为MMS应用程序启动的任务中的一个Activity，并位于该任务的顶层。如果它重新定位到联系人的任务中，则我们重新启动短信任务的时候就看不到这个拨号界面了。
 * Activity的亲和力是由taskAffinity属性定义的，Task的亲和力是通过读取当前任务根Activity的亲和力决定的。因此，根据定义，根Activity总是位于相同亲和力的任务里。由于在某些需求的要求下，一些Activity的启动模式（由launchModel属性定义）为singleTask和singleInstance，此类Activity只能位于任务的底部，因此，想要使用allowTaskReparenting属性来调整Activity所属任务，则启动默认只能限于”standard”和”singleTop”这两个模式。

2. android:alwaysRetainTaskState
该属性表明该Activity所在任务的状态是否由系统保存。
如果是，则其值为true，如果配置为false，则表示在一定情况下Android将以初始状态启动该任务。
默认值是false。
 * 注意，该属性仅对任务的根Activity起作用，其他的所有Activity都会被忽略。
 * 当用户重新选择显示该任务的时候，系统在通常情况下将会清理掉任务中除了根Activity外的其他Activity。这种情况通常是指用户在一定时间限制内未对该任务进行操作，例如30分钟内。反之如果该属性配置为true时，系统总会以任务的最后状态来显示该任务，而不管用户是如何返回的。

3. android:clearTaskOnLaunch
该属性表明，除了任务中的根Activity，其他所有Activity是否都将从任务中移出。
如果想要在启动时只保留根Activity，则设置这个属性的值为true，否则为false。
默认值是false。
 * 该属性仅对启动一个新任务的根Activity有意义。
 * 当配置为true时，每当用户再次启动任务时，则总是由任务的根Activity来处理请求。
 * 如果该属性和allowTaskReparenting都是true，则可重新成为父任务的任何Activity就要被移动到具有相同亲和力的任务上，接着保留的Activity就被销毁。

4. android:configChanges
在某些设备配置（比如屏幕方向，字体大小，网络类型等）发生变化的时候，Activity将会被重新启动以适配新的配置，这是系统行为。而Android同样为应用程序提供了一个阻止这种行为发生的手段，如果你不想因为某种配置变化而发生Activity重启，则可以通过配置这个属性并选择你想要阻止的配置。如果你配置完毕并选择了你关注的配置，则当这些配置发生改变的时候Activity不会重启，而是通过onConfigurationChanged()回调方法通知应用程序这些配置发生了变化。
 * 注意：如非必要，应该避免使用该属性
   所有这些配置的改变都能影响到应用程序对资源文件的选择。所以，当onConfigurationChanged（）被调用时，通常需要重新获取所有的资源（包括视图布局和图片等），以便正确地处理这些改变。
   **注意，如果我们没有实现onConfigurationChanged（）回调，那么该Activity就会被销毁并重新创建。**

5. android:enabled
该属性表示Activity是否能被实例化。
一般来说，每个Activity由Activity框架负责实例化，但你可以通过配置该属性来限制系统的这种行为。
为true表示由系统实例化，否则为false。
默认值是true。
对于每一个Activity的子类，在它首次运行之前总要进行实例化，这个步骤是必须的。
我们可以使用这个属性来控制Android框架实例化Activity的行为，~但这样做是有风险的，所以不建议你这样做~。

6. android:excludeFromRecents
Android框架为我们维护了一个名叫**“最近运行”的应用程序列表**，以方便进行应用程序切换。
该属性表示应用程序是否应该将Activity从最近运行的应用程序列表排除。
如果排除，则为true，否则为false。
默认值为false。
这个属性的前提是该Activity是某个任务的根Activity。

7. android:exported
该属性表示Activity是否可以由其他应用程序中的组件来启动。
如果可以，则为true，否则为false。
如果为false，则该Activity只能由同一应用程序的组件或者有同样用户ID的应用程序来启动。
**注意，如果你试图从你的应用程序中启动其他应用程序组件，在没有使用该属性的情况下，你必须以新任务（newTask）的方式启动。**

8. android:finishOnTaskLaunch
该属性是指不管何时，当用户再次启动Activity的任务时（在主页屏幕上选择该任务），
是否应销毁（或者终止）这个Activity的实例，
如果应销毁，则为true，否则为false.
默认值是false。

9. android:hardwareAccelerated
该属性是指是否应为该Activity启动硬件加速。
如果应启动，则为true，否则为false。
默认值是false。
 * 注意：不是所有的OpenGL 2D操作都会被加速。
   如果启用硬件加速渲染器，则要测试你的应用程序以便确保它能使用渲染器而不会产生错误。

10. android:icon
它代表Activity和图标。
在Activity被显示的时候，就用该图标显示给用户。
例如，用于示例任务的Activity的图标，或者桌面上的图标。
该属性** 必须 ** 设置为图片资源引用，如果没有设置，就使用< application />节点上的icon属性。

11. android:label
该属性用于描述该Activity的一个标签，通常是随着Activity图标一起显示出来的。
如果没有设置该属性，则使用< application />节点上的label属性设置的值。

12. android:launchMode
这个属性描述了该Activity应该如何被启动。
在Intent对象中，与Activity标志一起工作的模式有4种，
分别是：`standard` , `singleTop` , `singleTask` 和 `singleInstance`。
默认模式是standard。
模式有两类，
 * 一类是 standard 和 singleTop ，另一类是 singleTask 和 singleInstance 。
 * 有 standard 和 singleTop 启动模式的Activity可多次被实例化。
 * standard 是默认。系统总是在目标任务中创建Activity的一个新实例并且将intent按顺序放入到实例中。
 * singleTop 有条件的 。如果Activity的实例已经存在于目标任务的顶部，
 * 则系统通过调用onNewIntent()方法将intent发送到该实例上，而不是创建Activity的一个新实例。
 * singleTask 系统在新任务的根上创建该Activity。
 * 如果已经存在实例，则系统通过调用onNewIntent()方法来将intent发送到该Activity上，它允许在这个Activity为根的任务中创建新的Activity。
 * singleInstance 和 singleTask 一样，除了系统不启动任何其他Activity到持有实例的任务上，Activity总是单个的，而且是其任务的唯一成员。
 * 相反，singleTask和singleInstance这两种模式下的Activity只能启动一个任务它们一直待在Activity栈的根上。此外，设备一次只保存Activity的一个实例。
 * standard和singleTop模式只在一个方面上是不同的。在satndard模式下，每次都会实例化一个Activity新实例来响应这个Intent，每个实例处理一个intent。与此相似的是，singleTop模式下的Activity的新实例也可被创建来处理新的intent。但是，如果目标任务在其栈的顶部已经有Activity的一个实例，则会使用这个已经存在的Activity的实例来处理这个intent（回调onNewintent()方法），而不会创建一个新实例。在其他情况下，如果singleTop模式下的Activity的一个已存在实例在目标任务中而非栈的顶部，或者如果它在栈的顶部而非目标任务中，就会创建一个新实例并将它压倒Activity栈顶上。
 * singleTask和singleInstance模式也同样存在不同的启动特性。singleTask模式下的Activity允许其他Activity成为它的任务的一部分，它总是在自身任务的根上，但是其他Activity可以被启动到该任务中。另一方面，singleInstance模式下的Activity不允许其他Activity成为其任务的一部分，它是任务中唯一的Activity。如果它启动了另一个Activity，则该Activity就被分配到不同的任务上，好比FLAG_ACTIVITY_NEW_TASK在intent中一样。

13. android:multiprocess
该属性表示Activity的实例是否可以运行在启动它的组件所在的应用程序进程中。
如果可以，则为true，否则为false。
默认值是false。

14. android:name
该属性表示Activity的类名，它是Activity的子类。
其属性值应该是一个标准的Java类名（如com.example.liyuanjing.ManiActivity）。
我们也可以将其标识为类的缩写，比如名称的首字母是一个点（例如.ManiActivity），
那么它就被追加< manifest />元素指定的包名，
从而变成com.example.liyuanjing.ManiActivity（假设包名为com.example.liyuanjing）。
这点完全由系统完成，我们不需要关心这个过程的细节，但这个属性是必须配置的，并且不提供默认值。

15. android:noHistory
这个属性用于设置在用户离开该Activity，并且它在屏幕上不再可见的时候，是否应该从Activity的堆栈中删除。
如果应该删除，则为true，否则为false，默认值是false。
true意味着Activity将不会留下历史痕迹，它将不会为任务而在Activity栈中保留数据，
所以用户将不能返回到Activity上。

16. android:permission
表示的是权限名称。
如果startActivity()或者startActivityForResult()的调用者还没有被授予指定的权限，则启动失败。
如果该属性没有设置，则< application />元素的 permission 属性设置的权限就应该应用到Activity中。
如果这两个属性都没有设置，则Activity就不会被权限保护。

17. android:process
该属性表示该Activity运行的进程名称。
通常，应用程序的所有组件在为应用程序而创建的默认进程中运行。
< application />元素的process属性可以为所有组件设置一个不同的进程，
但是每个组件可以覆盖这个属性的值，这样就实现了将应用程序部署在多个进程间。
如果分配该属性的名称是以冒号（：）开头，则在需要新进程并且Activity在该进程中运行的时候，
就会创建一个对于应用程序私有的新进程。

18. android:screenOrientation
该属性表示Activity显示的方向（比如纵向，横向），它是值可以是下表中的任意一个字符。
取值 说明
 * unspecified 默认值，根据重力感应选择方向
 * user 用户当前偏好的方向
 * behind 和Activity相同的方向
 * landscape 横向
 * portrait 纵向
 * reverseLandscape 与正常横向相反方向 的横向
 * reversePortrait 与正常纵向相反方向的纵向
 * sensorLandscape 只能是横向，但是可以根据重力感应来决定是正常的还是反转的横向
 * sensorPortrait 中能是纵向，但是可以根据重力感应来决定是正常的或者反转的纵向
 * sensor 方向由设备方向感应器来决定。显示的方向取决于用户是如何持有设备的；在用户翻转设备时，方向发生改变。有些设备在默认情况下不会翻转到所有4个可能的方向。要允许可翻转到所有4个方向，可以使用fullSensor。
 * fullSensor 方向由设备方向感应为4个方向中的任意一个而确定
 * nosensor 无感应模式

19. android:stateNotNeeded
该属性表明Activity是否能被终止以及是否能在还没有保存其状态的情况下成功重启。
如果Activity可以在不需要引用到之前状态的情况下就能被重启，则该属性为true；
如果需要引用到之前的状态才能被重启，则为false。默认值是false。
 * 通常，在暂时关闭Activity之前，我们要调用onSaveInstanceState()方法来保存当前的Activity的状态。该方法在Bundle对象中存储Activity的当前状态，该对象在重启Activity时将会以参数的方式传给onCreate()方法。
 * 如果该属性被设置为true，则onSaveInstanceState()就不会被调用，并且onCreate()会被传递null，这和Activity首次启动时所做的一样。

20. android:taskAffinity
该属性指明对该Activity有亲和力的任务。
有同样亲和力的Activity在概念上属于同一任务（默认情况下是应用程序所定义的任务）。
任务的亲和力是由其根Activity的亲和力所决定的。

21. android:theme
该属性是指为Activity定义一个整体主题风格资源的引用。
所谓的风格包括字体种类，整体样式等。使用该属性可以使得我们的Activity在整体上更为统一，美观。
 * 如果没有设置该属性，则Activity继承将应用程序作为一个整体而设置的主题，具体可见元素的theme属性。如果theme属性也没有设置，则使用默认系统主题。

22. android:windowSoftInputMode
该属性表示Activity的主窗口如何与包含屏幕软键盘的窗口交互。
设置该属性将影响两件事。
 * 软键盘的状态。当Activity获取输入焦点时，是否隐藏软键盘。
 * 对Activity主窗口的调整。
   该窗口是否被调整得更小一些来为软键盘腾出空间，或者它的内容是否被移动以便在部分窗口被软键盘覆盖时，使得当前焦点可见。该属性或者是下表的一个值，或者是state…值和adjust…值的组合。如果是多个值的组合，则使用（|）将其隔开。

