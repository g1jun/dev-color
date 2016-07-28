---
title: swift-Image的AccessibilityIdentifier属性使用
date: 2016-07-28 14:19:14
前言:
---
有时候我们使用ImageView的时候 想要知道现在的显示的图片是placeHolder的图片还是加载完成或挑选好的图片，但是tag属性只能拿到却不能判断，当然，加几个bool属性也可以完成，但是会有些复杂，如果可以在给imageview 赋图片的时候给图片加个标记，那么下次判断一下标记是不是placeHolder就可以了，很简单。

#### <font color="blue">Blue Elves</font> 提供[Image的AccessibilityIdentifier属性使用](http://blog.csdn.net/tutuzhuz/article/details/52055404)技术支持

    
<!-- more -->
## 代码：
``` swift

let picView = UIImageView(frame: CGRectMake(10, 90, 50, 50))
        picView.layer.cornerRadius = 3
        picView.userInteractionEnabled = true
        picView.layer.masksToBounds = true
        picView.image = UIImage(named: "add_pic.png")
        picView.image?.accessibilityIdentifier = "add"
        
```

这个图片 “add_pic.png” 的标记就是  “add” ，当你更换picview的image时，如果不给AccessibilityIdentifier属性重新赋值的话，这个属性的值就会变成null，每个图片都会对应一个专属的AccessibilityIdentifier；方便我们识别图片。
    

``` swfit 

 // 判断
        if picView.image?.accessibilityIdentifier == "add" {
            //是默认图、替换图片
            self.chooseImaeFromAblum({ (image) -> (Void) in
                picView.image = image
                picView.accessibilityIdentifier = "new"
            })
        }else{
            //（ImageView中不是默认图）查看大图 或进行 其他操作
        }
        
```

``` oc
// oc
UIImageView * picView = [[UIImageView alloc] initWithFrame:CGRectMake(10, 90, 50, 50)];
    picView.layer.cornerRadius = 3;
    picView.userInteractionEnabled = YES;
    picView.layer.masksToBounds = YES;
    picView.image = [UIImage imageNamed:@"add_pic.png"];
    [picView.image setAccessibilityIdentifier:@"add”];

if ([picView.image.accessibilityIdentifier isEqualToString:@"add"]) {
       [self chooseImageFromAblum:^(UIImage *image) {
        picView.image = image;
// 给新图片的AccessibilityIdentifier赋新值
        [picView.image setAccessibilityIdentifier:@"new"];
    }]; 
    }else{
        [self blowUpImageWithPic:picView.image];
    }

```

这样会方便很多，减少bool变量过多带来的问题。

