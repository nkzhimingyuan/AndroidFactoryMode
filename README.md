# AndroidFactoryMode
go into the factory mode of Android by programming 
# Android手机通过程序进入工程模式


本文将介绍如何编写程序进入Android 手机的工程模式。


###背景介绍
因为需要查看手机里面和信号有关系的信息，所以我们需要进入Android手机里面的工程模式，去查看里面的信息。很多方式都是使用通过手机拨打一串字符进入的，但是我们需要在程序中读取这些数据，所以我们需要用代码实现。下图是进入到工程模式中查看到的手机的信息：

![这里写图片描述](http://img.blog.csdn.net/20150409091832316)



###相关调研

经过我的观察发现，工程模式其实是Android手机“设置”里面的一个选项罢了，但是被隐藏了而已，所以我们去看Android 中和设置相关的源代码，在com.android.settings 这个文件夹下发现了一个名为“TestSettings.java”的文件：

![这里写图片描述](http://img.blog.csdn.net/20150409091815577)

这里面显示需要加载一个R.xml.testing_settings文件，而且该java文件没有任何其他的动作，那么我们来看下这个R.xml.testing_settings文件。

![这里写图片描述](http://img.blog.csdn.net/20150409091758276)

我们看到这个文件中又包含了几个targetClass，并且通过名字，我们可以看到这是和工程模式的界面相对应的。

![这里写图片描述](http://img.blog.csdn.net/20150409091744845)

那么我们现在需要得到RadioInfo.java 里面的信息，我们就可以直接在源码中去查找就可以了。


###实现代码
```	java
    private static final String               TAG             = SignalHelpers.class.getSimpleName();
    public static final  String               INTENT_ACTION   = "android.intent.action.MAIN";
    public static final  Pair<String, String> SETTINGS_INTENT =
        Pair.create("com.android.settings", "TestingSettings");

    /**
     * Get the intent that launches the additional radio settings screen
     *
     * @return the intent for the settings area
     *
     * @throws SecurityException the security exception
     */
    public static Intent getAdditionalSettings() throws SecurityException
    {
        Intent intent = new Intent(Intent.ACTION_VIEW);
        ComponentName showSettings = new ComponentName(
            SETTINGS_INTENT.first,
            String.format("%s.%s",
                SETTINGS_INTENT.first,
                SETTINGS_INTENT.second));
        return intent.setComponent(showSettings);
        
    }
```

###总结
这篇文章主要是讲述如何通过编程进入Android手机中的工程模式，并且可以获得工程模式中的相关信息。

