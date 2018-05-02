# Cocos2dx 设置设计分辨率

我们以 1136 * 640 作为设计分辨率（在Cocos2dx Studio中也一样设置）
则程序中，需要设置设计分辨率，使得能够适配所有的分辨率（包括iphoneX）
当我们用mac桌面版来进行调试的时候，需要先设置mac桌面版的分辨率。

需要注意的是：mac桌面版是没有状态栏的

``` c++

    auto director = Director::getInstance();
    auto glview = director->getOpenGLView();
    Size framesize  = Director::getInstance()->getWinSize();
#if(CC_TARGET_PLATFORM == CC_PLATFORM_MAC)
    Size sizeIPad   = Size(1024, 768);
    Size sizeIPhX   = Size(2436/2.0, 1125/2.0);
    Size sizeNomal  = Size(1136, 640);
    framesize       = sizeIPhX;
#endif
    CCLOG("frameSize w = %f, h = %f", framesize.width, framesize.height);
    ResolutionPolicy policy = ResolutionPolicy::FIXED_WIDTH;
    float fW = 1136, fH = 640;
    float scaleX = framesize.width / fW, scaleY = framesize.height / fH;
    float width = framesize.width / scaleX, height = framesize.height / scaleX;
    if(framesize.width / framesize.height > fW/fH){
        policy = ResolutionPolicy::FIXED_HEIGHT;
        width  = framesize.width / scaleY;
        height = framesize.height / scaleY;
    }
    
    if(!glview) {
        glview = GLViewImpl::createWithRect("myGames", Rect(0, 0, framesize.width, framesize.height));
        director->setOpenGLView(glview);
    }
    director->getOpenGLView()->setDesignResolutionSize(width, height, policy);
``` c++
