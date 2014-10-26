---
layout: post
title: "cocos2d-x CCNode"
description: "cocos2d-x CCNode"
category: cocos2d-x
tags: [cocos2d-x, CCNode]
---

##cocos2d-x节点类CCNode

---
**转载自：[http://blog.csdn.net/jackystudio/article/details/12703741](http://blog.csdn.net/jackystudio/article/details/12703741)**

---
节点类CCNode可以说是游戏元素的祖宗了，基本上我们看得到的游戏元素都是以它为原型进行扩展的。像CCScene，CCLayer，CCSprite，CCMenu，CCSpriteBatchNode等等都是从CCNode继承而来。另外如果我们要自定义精灵，那么从CCNode继承也是一个很不错的选择。    

###1.概况

{:.center}
![CCNode](/images/CCNode.jpg)  

**CCNode直接从CCObject继承而来，有如下几个特点：**

--- 
（1）可以包含其他CCNode节点，可以进行添加/获取/删除子节点操作。  
（2）可以执行周期性的回调任务。  
（3）可以执行动作。  

---
  
一些子类化的节点提供了更为丰富的特性和功能。  

###2.属性
 Features of CCNode:
{% highlight cpp %}
 - position                                   //位置，默认(0,0)
 - scale (x, y)                               //缩放，默认(1,1)
 - rotation (in degrees, clockwise)           //旋转，默认为0
 - skew                                       //倾斜，默认为0
 - CCCamera (an interface to gluLookAt )      //CCCamera，视点转换，每个节点都有，默认指向节点中心
 - CCGridBase (to do mesh transformations)    //CCGridBase，网类转变
 - anchor point                               //锚点，默认(0,0)
 - size                                       //尺寸，默认(0,0)
 - visible                                    //可见
 - z-order                                    //z轴值
 - openGL z position                          //OpenGL z值
{% endhighlight %}

###3.接口

####3.1初始化
{% highlight cpp %}
	//初始化函数，成功返回true
	virtual bool init();
	//分配内存空间，调用init并添加autoRelease标记
	static CCNode * create(void);
	//返回描述字符串
	const char* description(void);
{% endhighlight %}

####3.2.图形属性
{% highlight cpp %}
	//设置/获取Z轴顺序，Z轴大的覆盖Z轴小的
    virtual void setZOrder(int zOrder);
    virtual void _setZOrder(int z);
    virtual int getZOrder();

    //设置/获取OpenGL Z轴顶点
    virtual void setVertexZ(float vertexZ);
    virtual float getVertexZ();

    //设置/获取缩放值
    virtual void setScaleX(float fScaleX);
    virtual float getScaleX();
    virtual void setScaleY(float fScaleY);
    virtual float getScaleY();
    virtual void setScale(float scale);
    virtual float getScale();
    virtual void setScale(float fScaleX,float fScaleY);

    //设置/获取位置
    virtual void setPosition(const CCPoint &position);
    virtual const CCPoint& getPosition();
    virtual void setPosition(float x, float y);
    virtual void getPosition(float* x, float* y);
    virtual void  setPositionX(float x);
    virtual float getPositionX(void);
    virtual void  setPositionY(float y);
    virtual float getPositionY(void);
    
    //设置/获取倾斜角度
    virtual void setSkewX(float fSkewX);
    virtual float getSkewX();
    virtual void setSkewY(float fSkewY);
    virtual float getSkewY();

    //设置/获取锚点
    virtual void setAnchorPoint(const CCPoint& anchorPoint);
    virtual const CCPoint& getAnchorPoint();
    virtual const CCPoint& getAnchorPointInPoints();
    
    //设置/获取大小
    virtual void setContentSize(const CCSize& contentSize);
    virtual const CCSize& getContentSize() const;

    //设置/获取可见性
    virtual void setVisible(bool visible);
    virtual bool isVisible();

    //设置/获取旋转角度
    virtual void setRotation(float fRotation);
    virtual float getRotation();
    virtual void setRotationX(float fRotaionX);
    virtual float getRotationX();
    virtual void setRotationY(float fRotationY);
    virtual float getRotationY();
{% endhighlight %}

####3.3.节点操作
{% highlight cpp %}
	//添加/获取子节点，可以带Z轴顺序（默认为0）和标签
    virtual void addChild(CCNode * child);
    virtual void addChild(CCNode * child, int zOrder);
    virtual void addChild(CCNode* child, int zOrder, int tag);
    CCNode * getChildByTag(int tag);
    virtual CCArray* getChildren();
    unsigned int getChildrenCount(void) const;
    
    //设置/获取父节点
    virtual void setParent(CCNode* parent);
    virtual CCNode* getParent();
    
    //从父节点中移除自身，默认cleanup为true
    virtual void removeFromParent();
    virtual void removeFromParentAndCleanup(bool cleanup);

    //移除子节点
    virtual void removeChild(CCNode* child);
    virtual void removeChild(CCNode* child, bool cleanup);
    virtual void removeChildByTag(int tag);
    virtual void removeChildByTag(int tag, bool cleanup);

    //移除所有节点
    virtual void removeAllChildren();
    virtual void removeAllChildrenWithCleanup(bool cleanup);
    
    //重新设置节点顺序
    virtual void reorderChild(CCNode * child, int zOrder);
{% endhighlight %}

####3.4.标签和用户数据
{% highlight cpp %}
	//添加/获取子节点，可以带Z轴顺序（默认为0）和标签
    //设置/获取tag
    virtual int getTag() const;
    virtual void setTag(int nTag);
    
    //设置/获取userdata，它是一个指针可以指向你想要的任意数据块，不过记得要释放
    virtual void* getUserData();
    virtual void setUserData(void *pUserData);
    
    //设置/获取CCObject，和上面一样，只是数据换成了CCObject对象
    virtual CCObject* getUserObject();
    virtual void setUserObject(CCObject *pUserObject);
{% endhighlight %}

####3.5.事件回调
{% highlight cpp %}
	//事件回调
    
    //节点开始进入触发
    virtual void onEnter();
    //节点完成进入触发
    virtual void onEnterTransitionDidFinish();
    //节点退出触发
    virtual void onExit();
    //如果节点退出有过渡动画，动画开始时触发
    virtual void onExitTransitionDidStart();
    //停止动画和调度器
    virtual void cleanup(void);
{% endhighlight %}

####3.6.动作
{% highlight cpp %}
	//获取/设置动作管理器
    virtual void setActionManager(CCActionManager* actionManager);
    virtual CCActionManager* getActionManager();
    
    //运行动作
    CCAction* runAction(CCAction* action);

    //停止动作
    void stopAllActions(void);
    void stopAction(CCAction* action);
    void stopActionByTag(int tag);
    CCAction* getActionByTag(int tag);

    //获取正在运行动作数
    unsigned int numberOfRunningActions(void);
{% endhighlight %}

####3.7.调度器和定时器
{% highlight cpp %}
//获取/设置调度器
    virtual void setScheduler(CCScheduler* scheduler);
    virtual CCScheduler* getScheduler();
    
    //检测某个调度器是否有在运行
    bool isScheduled(SEL_SCHEDULE selector);

    //开启update调度
    void scheduleUpdate(void);
    //设置调度优先级
    void scheduleUpdateWithPriority(int priority);
    //关闭update调度器
    void unscheduleUpdate(void);

    //开启/关闭/恢复/暂停调度器
    void schedule(SEL_SCHEDULE selector, float interval, unsigned int repeat, float delay);
    void schedule(SEL_SCHEDULE selector, float interval);
    void scheduleOnce(SEL_SCHEDULE selector, float delay);
    void schedule(SEL_SCHEDULE selector);
    void unschedule(SEL_SCHEDULE selector);
    void unscheduleAllSelectors(void);
    void resumeSchedulerAndActions(void);
    void pauseSchedulerAndActions(void);
    
    //每帧调用函数
    virtual void update(float delta);
{% endhighlight %}

####3.8.坐标转换
{% highlight cpp %}
    //坐标转换相关，这一部分后面再介绍
    CCPoint convertToNodeSpace(const CCPoint& worldPoint);
    CCPoint convertToWorldSpace(const CCPoint& nodePoint);
    CCPoint convertToNodeSpaceAR(const CCPoint& worldPoint);
    CCPoint convertToWorldSpaceAR(const CCPoint& nodePoint);
    CCPoint convertTouchToNodeSpace(CCTouch * touch);
    CCPoint convertTouchToNodeSpaceAR(CCTouch * touch);
{% endhighlight %}

####3.9.其它
{% highlight cpp %}
    //获取/设置着色程序
    virtual CCGLProgram* getShaderProgram();
    virtual void setShaderProgram(CCGLProgram *pShaderProgram);
    
    //获取CCCamera对象
    virtual CCCamera* getCamera();
    
    //节点是否在运行
    virtual bool isRunning();

    //绘制节点
    virtual void draw(void);
    //递归访问节点
    virtual void visit(void);

    //返回所占矩形，节点坐标系
    CCRect boundingBox(void);
{% endhighlight %}

###4.CCNodeRGBA

{:.center}
![CCNOdeRGBA](/images/CCNOdeRGBA.jpg)

CCNodeRGBA继承于CCNode，所以它拥有CCNode的所有特性，并且它也继承于CCRGBAProtocol。从名字看来我们就知道它是一个带有颜色和透明度的节点。所以它比起CCNode就多了2个特性，Opacity和RGB值。如果要给子节点传递透明度属性，那么需要设置setCascadeOpacityEnabled(true)，如果传递的过程中遇到了CCNode，那么传递会中断。颜色值的传递也是一样的道理。
  
  