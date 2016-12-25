# 有道云笔记去除左下角广告

找到有道云笔记的安装路径，`*\Youdao\YoudaoNote\theme\build.xml`
用笔记本打开这个文件，找到‘左下角广告’这几个字，把下面的代码删掉：
```xml
<PanelAd type="adpanel" css="public" ass="mainform panelclient PanelAd">
    <panelTopLine type="panel" css="AdPanel" Dockstyle="top" Bounds="0,0,0,1"/>
    <AdPhoto type="photo" css="Ad AdPhoto" ass="common fill"/>
    <AdText type="label" css="AdText" AnchorStyle="topleft" Bounds="20,135,25,10" Margin="0,0,0,0"/>
</PanelAd>
```
之后保存，重启有道云笔记，就可以了。
