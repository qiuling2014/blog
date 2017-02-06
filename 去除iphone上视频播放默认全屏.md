最近做项目时，有个需求是手机上播放视频要默认屏播放，不要自动全屏，经过测试，在chrome浏览器上是ok的，qq浏览器会调用自己的播放器（这个播放器还有发射弹幕的功能，NB呀），在iPhone上是会自动默认全屏播放，iPad上点播放不会自动全屏，是默认原来大小的。

论坛中搜索，都没给出答案，google —> stackoverflow，找到最终解决方法：

	HTML里video必须加上webkit-playsinline属性
    <video id="player" width="480" height="320" webkit-playsinline>
