# Understanding constraints
![example code](./images/article-hero-image.png)

* 對話順序 
    * 從parent得到constraints
    * 將計算過後的constraint一個一個給child並一個一個得到該child size
    * 決定每一個child位置
    * 回覆parent所需size

![example code](./images/children.png)

* Limitations
    * 每個widget不能自己決定size
    * 不知道自己在螢幕中的座標

* widget如何處理constraint
    * 由widget's RenderBox 處理
    * Those that try to be as big as possible. For example, the boxes used by Center and ListView.
    * Those that try to be the same size as their children. For example, the boxes used by Transform and Opacity.
    * Those that try to be a particular size. For example, the boxes used by Image and Text.
    * Row / Column 這種flex box處理不同constraint有不同處理方式.
        * 在primary direction方向上是bounded constraint, 那就盡量大。
        * 在unbounded constraint, 那就fit child widget, 並且不能使用Expanded在another flex box或scrollable下.
    * Container
    * FittedBox

