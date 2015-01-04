---
layout: post
title:  "iOS键盘遮挡问题解决（Swift版）"
date:   2015-01-04 11:08:18
categories: jekyll update
tag: iOS

---
刚接触iOS开发，发现与Android最大不同的地方就在于iOS需要处理键盘遮挡文本输入框以及单击屏幕空白处需要键盘消失两种事件。  
后来仔细一想，确实也有道理，Android因为碎片化太严重，如果让开发者自己做这件事，估计还没开始写代码就跪了。然后键盘消失，因为Android设备一般有返回键，所以一般不需要处理键盘消失这种问题。  
OK，回到主题上来，iOS键盘遮挡问题解决  
这个问题网上有很多解决方案，但很多要不效果不好，要不就太繁琐，最后找到了一种方法，大致如下：  
keyboard的出现与UITextField密切相关，可以通过实现UITextField的三个委托方法，来解决键盘遮挡问题

实现方法大致如下
实现UITextFieldDelegate 三个方法  textFieldDidBeginEditing  textFieldShouldReturn textFieldDidEndEditing
{% highlight ruby %}   
	func textFieldDidBeginEditing(textField: UITextField) {
    let frame = textField.frame
    var offset: CGFloat
    if let navigationHeight = self.navigationController ? .navigationBar.frame.size.height {
        offset = frame.origin.y + frame.height * 2 - (self.view.frame.size.height - keyboardHeight - navigationHeight)
    } else {
        offset = frame.origin.y + frame.height * 2 - (self.view.frame.size.height - keyboardHeight)
    }
    let animationDuration: NSTimeInterval = 0.3 UIView.beginAnimations("ResizeForKeyboard", context: nil) UIView.setAnimationDuration(animationDuration)
    if offset > 0 {
        self.view.frame = CGRectMake(0, -offset, self.view.frame.width, self.view.frame.height + offset)
    }
    UIView.commitAnimations()
}
{% endhighlight %}