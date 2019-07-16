# Label综述

Label是游戏中的重要元素，特别是目前国内手游更是需要大量的文字描述来告诉玩家各种信息，比如玩法规则，物品属性，玩家属性等。了解Label的实现原理，能对游戏性能提升有所帮助。

## 创建

```cpp
//
    static Label* createWithTTF(const TTFConfig& ttfConfig, const std::string& text, 
        TextHAlignment hAlignment = TextHAlignment::LEFT, int maxLineWidth = 0);

//
    static Label* createWithBMFont(const std::string& bmfontPath, const std::string& text,
        const TextHAlignment& hAlignment = TextHAlignment::LEFT, int maxLineWidth = 0,
        const Vec2& imageOffset = Vec2::ZERO);

//
    static Label * createWithCharMap(Texture2D* texture, int itemWidth, int itemHeight, int startCharMap);



```



## 设置字体

```cpp
//
virtual bool setTTFConfig(const TTFConfig& ttfConfig);

//
virtual bool setBMFontFilePath(const std::string& bmfontFilePath, const Vec2& imageOffset = Vec2::ZERO, float fontSize = 0);

//
virtual bool setCharMap(Texture2D* texture, int itemWidth, int itemHeight, int startCharMap);

```



## 显示









