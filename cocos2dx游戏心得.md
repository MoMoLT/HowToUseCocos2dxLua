[TOC]

# 控件运动

------

## 动作

```
# 一次动作
local rundown = cc.MoveBy:create(FISHHOOKSPEED[1], cc.p(0, FISHHOOKSPEED[2] * self.fishhookDirection))
    self.fishhook:runAction(rundown)
    
# 反复动作
local run11 = cc.MoveBy:create(FLOATSPEED[1],cc.p(0 , FLOATSPEED[2]))
local run12 = cc.MoveBy:create(FLOATSPEED[1],cc.p(0 , -FLOATSPEED[2]))
local delay = cc.DelayTime:create(0.01)    self.fishhookButton1:runAction(cc.RepeatForever:create(cc.Sequence:create(run11,delay,run12,delay)))

# 停止动作
self:stopAllActions()
```



## cc.Sequence/runAction中插入其他函数

```
local run1 = cc.RotateBy:create(toLeftRotation * speedUnit, toLeftRotation)
local run2 = cc.RotateBy:create(toLeftRotation * speedUnit, -toLeftRotation)
local change = CCCallFuncN:create(self.ChangeDirection) -- 这是自己写的函数

self.fishhook:runAction(cc.RepeatForever:create(cc.Sequence:create(run1, run2, change)))

function GameScene:ChangeDirection()
    FISHSWAYDIRECTION = -FISHSWAYDIRECTION
end
```

## 渐入/渐出

```
sprite:runAction(cc.FadeOut:create(0.5))		# 出, 0.5s时间
sprite:runAction(cc.FadeIn:create(0.5))			# 入
```



## 旋转

```
self.sprite:runAction(cc.RepeatForever:create(cc.RotateBy:create(1,360))) --以锚点为中心不停的旋转，速度为每秒中360度

self:setRotationSkewX(90)
self:setRotationSkewY(90)
```



## 角度公式换算

```
-- 设置角度
setRotationSkewX(36.33)
setRotationSkewY(36.33)
setRotation(30)
-- 获取角度
getRotation()
-- 角度换算
math.rad(30)			-- 角度转化弧度
math.sin(math.rad(30))	 -- 正弦计算
math.cos(math.rad(30))	 -- 余弦计算
math.deg(1)				-- 弧度转化角度
math.deg(math.asin(0.5)) -- 反正弦
```



## 翻转

```
Ship:setFlippedX(true)			-- 左右翻转
ship:setFlippedY(true)			-- 上下翻转
```



# 控件参数[位置，大小，透明度，节点]

------

## 加入或移除控件

```
-- 对于父节点
self:addChild(控件)
self:removeChild(控件)
self:removeAllChildren()	-- 移除所有子节点

-- 对于子节点
控件:addTo(父节点)
控件:removeFromParent()
```

## 获取父节点下的子节点操作

```
-- 获取所有子节点， 返回值是列表
父控件:getChildren()

父控件:removeAllChildren()		-- 移除所有子节点
```



## 获取长宽

```
-- 这个是获取原大小，非改变后的
fish:getContentSize().width
fish:getContentSize().height

-- 据说这个才是最好的
rect = button:getBoundingBox()		-- 获得一个矩形
rect.x, rect.y, rect.width, rect.height
```



## 设置锚点

```
sprite:setAnchorPoint(x, y)
```



## 获取位置

```
-- 控件位置，即控件锚点的位置
local x, y = self.fishhookButton1:getPosition()
self.fish:setPosition(x, y)
```



## 世界坐标与本地坐标换算

```
**// 把世界坐标转换到当前节点的本地坐标系中 
Point convertToNodeSpace(cc.p(x, y)) const;**

**// 把基于当前节点的本地坐标系下的坐标转换到世界坐标系中 
Point convertToWorldSpace(cc.p(x, y)) const;**

**// 基于Anchor Point把世界坐标转换到当前节点的本地坐标系中 
Point convertToNodeSpaceAR(cc.p(x, y)) const;**

**// 基于Anchor Point把基于当前节点的本地坐标系下的坐标转换到世界坐标系中
Point convertToWorldSpaceAR(cc.p(x, y)) const;**

# 使用方法, size为按钮大小与宽高, self.levelType1Frame为button父节点
# 把button的左下角的坐标映射到全局坐标里
# 即button的当前坐标为self.levelType1Frame里的局域坐标， 即坐标系是以self.levelType1Frame建立的，然后把映射到以背景为坐标系的坐标上
local size = button:getBoundingBox()
local point = self.levelType1Frame:convertToWorldSpace(cc.p(size.x, size.y))
print(point.x, point.y)
```



## 控件的可见/不可见

```
控件对象:setVisible(false)	-- 不可见
控件对象:setVisible(true)	-- 可见
```



## 透明度

```
sprite:getOpacity()			-- 获取透明度
sprite:setOpacity(100)		-- 设置透明度
```



## 设置Label文字

```
ScoreLabel:setString("123")
```



## 按钮设置字体大小

```
Button:setTitleFontSize(14)
```



# 控件其他操作

------

## 画直线

```
self.Yg_DrawNode = cc.DrawNode:create()
self:addChild(self.Yg_DrawNode)
self.Yg_DrawNode:drawLine(cc.p(qidian_x,qidian_y),cc.p(zhongdian_x ,zhongdian_y),cc.c4f(0,0,0,5)) --参数 起点坐标，终点坐标，线条的颜色
```



## 创建一个图片

```
-- 这个不常用嘛
local image = ccui.ImageView:create()
image:ignoreContentAdaptWithSize(false)             -- 开启自定义大小
-- 利用了合图
--cc.SpriteFrameCache:getInstance():addSpriteFrames(PLISTPATH)
image:loadTexture(imagePath)

local layout = ccui.LayoutComponent:bindLayoutComponent(image)
layout:setSize({width = size[1], height = size[2]})

image:setAnchorPoint(0.5, 0.5)
```



## 更换Sprite图片

```
self.player:setSpriteFrame('xxxx/xxx/1.png')
```



## 鼠标响应事件，获取鼠标位置

```
-- 在onCreate()中
-- 一个响应鼠标点击的容器
self.event_layout = ccui.Layout:create():addTo(self)
self:getMousePos()

-- 创建函数
-- 获取鼠标位置
function LevelScene:getMousePos()
    print("sdf")
    -- 鼠标点击
    local function onTouchBegan(touch, event)
        return true
    end
    -- 鼠标不松开移动
    local function onTouchMoved(touch, event)
    end
    -- 鼠标松开
    local function onTouchEnded(touch, event)
        local location = touch:getLocation()
        local event_x = location["x"] or 0
        local event_y = location["y"] or 0
        print("event_x="..event_x.." event_y="..event_y)
    end

    local listener = cc.EventListenerTouchOneByOne:create()
    listener:registerScriptHandler(onTouchBegan, cc.Handler.EVENT_TOUCH_BEGAN)
    listener:registerScriptHandler(onTouchMoved, cc.Handler.EVENT_TOUCH_MOVED)
    listener:registerScriptHandler(onTouchEnded, cc.Handler.EVENT_TOUCH_ENDED)

    local eventDispatcher = self.event_layout:getEventDispatcher()
    eventDispatcher:addEventListenerWithSceneGraphPriority(listener, self.event_layout)
end
```



## 判断点在矩形中

```
local buttonRect = button:getBoundingBox()        -- 获取按钮左下角位置，宽高
local point = self.levelType1Frame:convertToWorldSpace(cc.p(size.x, size.y))   -- 获取按钮全局坐标
local rect = cc.rect(point.x, point.y, width, height)
-- 获取按钮坐标
if(cc.rectContainsPoint(rect, self.location)) then
	print("ok==================")
end
```



# 框架

------

## 载入场景代码

```
local MainScene = class("MainScene", cc.load("mvc").ModuleBase)
-- class("MainScene"...)中，MainScene是自己定义的类名，可以随意写

MainScene.RESOURCE_FILENAME = "game/Fishing/MainScene.csb"
-- "game/Fishing/MainScene.csb"是场景资源位置
```



## 读取场景中的控件

```
function MainScene:onCreate()
    print("你进入了登陆界面")
    self.help = self.mView["ButtonHelp"]
    
    print(self.help)
end

-- 其中self.mView是场景中的控件集合，["ButtonHelp"]为控件标签名，是自己命名的
```



## 按钮绑定

```
-- 这是普通的绑定
local ButtonClose = self.mView["close"]
        :addTouchEventListener(function(sender, eventType)
            self:setVisible(false)
        end)
        
-- 公司框架绑定
GameScene.btnBind = {
    PauseButton     = "pauseButtonClick",
}
-- 这里的PauseButton变量名就是控件标签名，是自己在cocos studio中命名的

-- 暂停按钮绑定函数
function GameScene:pauseButtonClick()
    self:playButtonSound()
    self.pauseFrame:setVisible(true)
    self.timeFrame:setVisible(true)
    --self.referenceImage:setVisible(true)
    self.director:pause()
end
```



## 定时器，帧更新

```
self:scheduleUpdateWithPriorityLua(function(dt)
        self:update(dt)
    end, 0)
# 再创建一个update函数，即每一帧都执行Update函数

# 或者
local scheduler=cc.Director:getInstance():getScheduler()
        :scheduleScriptFunc(self.update,1, false)
# 可以定时1s

# 公司框架
self:onUpdate(handler(self, self.update))               -- 定时器启动
function xxxScene:update(dt)
	xxxx
end
```



## 场景暂停

```
self.sharedDirector = cc.Director:getInstance()     -- 导演负责游戏开始与结束
self.sharedDirector:pause()						  -- 暂停
self.sharedDirector:resume()					  -- 恢复
```



## 场景切换

```
-- 切换其他场景
local LevelScene = require("logic.game.jigsaws.LevelScene")
local levelScene = LevelScene:create():showWithScene()


-- 回主界面
local ViewJump = require("logic.common.views.viewJump")
ViewJump.gotoMainGame()
```



## 数据保存到文件

```
local fileSave = require("logic.common.tools.SaveTools")
-- 保存的形式为字典形式 {"classicLevelState":{123,123}}
-- 获取数据，无该键，则为空
levelState = fileSave.getData("ClassicLevelState")
-- 保存数据levelState为要保存的值，"ClassicLevelState":以改键对应该值
fileSave.saveData("ClassicLevelState", levelState)
```



## 公司框架之倒计时countDown

```
-- 创建一个倒计时计时器timer,时间10s
global.countDown.createTimer("timer", 10)
-- timer绑定控件referenceImage, 每秒执行function()
global.countDown.bindTimer(self.referenceImage, "timer", function()
        print(os.time())
    end)
    
-- 想获取还剩多少时间
self.remainTime = 10
global.countDown.createTimer("timer", self.remainTime)
global.countDown.bindTimer(self.referenceImage, "timer", function()
        print(self.remainTime)
    end)
```



# 其他操作

------

## 三元式

```
x = 1==0 and 1 or 0			-- 1==0是：1 否：取0
```



## 随机数

```
math.randomseed(os.time())		-- 种子生成
math.random(1, 100)				-- [1,100]内随机数值

```



## 数据类型转换

```
-- 数字转字符
tostring(num)
-- 字符转数字
tonumber(string)

```



## 列表与字典操作

```
-- 取列表list长度
	#list
-- 移除列表中某个参数
table.remove(列表名, id)
-- 插入
table.insert(self.fishSet, fish)

-- 一个字典
local dict = {["key"] = value, }

```



## 格式化输出

```
print(string.format("%02d-%02d-%02d"), 12, 1, 2)
-- 12-01-02

```



## 颜色参数

```
RGB -- 以下常用的
香蕉黄：236, 169, 0
灰黑：	69，69，69

```



# 声音

------

## 声音

```
-- 全局参数
local BGMUSIC       = "game/Marine/audios/bgm.mp3"
local BUTTONSOUND   = "game/Marine/audios/button.mp3"

-- 资源预加载
audio.preloadMusic(BGMUSIC)
audio.preloadSound(BUTTONSOUND)
-- 播放背景音乐
audio.playMusic(BGMUSIC, true)
-- 暂停音乐
audio.stopMusic(false)
-- 播放按钮等音效
AudioEngine.playEffect(BUTTONSOUND, false)

```



