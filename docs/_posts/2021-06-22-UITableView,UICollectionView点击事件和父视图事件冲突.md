---
layout: post
title:  "UITableView,UICollectionView点击事件和父视图事件冲突.md"
date:   2021-06-22
categories:
 - Turorial
tags:
 - Skills
---


UITableView,UICollectionView与外层View事件冲突，在外层View上添加了手势事件，然后在collectionView的协议方法didSelectItemAtIndexPath就不会响应, 只会响应外层View的手势事件。解决方法如下所示:



```
UITapGestureRecognizer *tapGestureRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(cancel)];
tapGestureRecognizer.delegate = self;
[self addGestureRecognizer:tapGestureRecognizer];
```

然后再collectionview的俯视图中实现下面的代理方法:

```
-(BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldReceiveTouch:(UITouch *)touch{
  if([touch.view isDescendantOfView:self.collectionView]){
    return NO;
  }
  return YES;
}
```

