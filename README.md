
# wx-bilibili

哔哩哔哩干杯🍻,我在小破站学前端之微信小程序


使用接口：https://easy-mock.com/mock/5c1dfd98e8bfa547414a5278/bili/navList

## index页面

index.json

```
{

	"navigationBarTitleText":"bilibili"

}
```

### 公共的头部

- 新建components文件夹>myTitle文件夹  右键新建component，取名为myTitle。

- 让它在首页 index 页面显示出来

  index.json

  ```
  {
  	"navigationBarTitleText":"bilibili",
  	"usingComponents":{
  	//要使用组件名称   //组件的路径
  	"MyTitle":"../../components/Mytitle/MyTitle"
  	}
  ```

  index.wxml

  ```
  <view class="main">
  	<!-- 公共头部 -->
      <MyTitle><MyTitle>
  </view>
  ```

  MyTitle.wxml

  ```
  <navigator class='logo'>
      <image class='logo_img' src='../../icons/logo.png'></image>
    </navigator>
  ```

### 首页数据加载

注意： 报错http：//....不在以下request合法域名列表中，参考文档！

可以在开发工具中点击 右上角详情，勾选 不校验合法域名。

```
//index.js
//获取应用实例
const app = getApp()

Page({
  data: {
    currentIndexNav:0,
   //首页导航数据
   navList:[],

   //轮播图数据
   swiperList:[],

   //视频列表数据
   videosList:[]
  },

  activeNav(e){
    //console.log(123);
    this.setData({
      currentIndexNav:e.target.dataset.index
    })
  },

  getNavList(){
    let that = this;
    wx.request({
      url: 'https://easy-mock.com/mock/5c1dfd98e8bfa547414a5278/bili/navList',
      success(res){
        //console.log(res)
       if(res.data.code===0){
         that.setData({
           navList:res.data.data.navList
         })
       }
      }
    })
  },
  getSwiperList(){
    let that=this;
    wx.request({
      url: 'https://easy-mock.com/mock/5c1dfd98e8bfa547414a5278/bili/swiperList',
      success(res){
        //console.log(res);
        if(res.data.code===0){
          that.setData({
            swiperList:res.data.data.swiperList
          })
        }
      }
    })
  },
  getVideosList(){
    let that = this;
    wx.request({
      url: 'https://easy-mock.com/mock/5c1dfd98e8bfa547414a5278/bili/videosList',
      success(res) {
        console.log(res);
        if (res.data.code === 0) {
          that.setData({
            videosList: res.data.data.videosList
          })
        }
      }
    })
  },
  onLoad: function () {
    this.getNavList();
    this.getSwiperList();
    this.getVideosList();
  },
  onReady:function (){

  }
})
```

index.wxml

```
<scroll-view class='nav' scroll-x> 
//bindtap 绑定点击事件
      <view bindtap="activeNav" data-index='{{index}}' 
      class="nav_item {{index===currentIndexNav?'active':''}}" 
      wx:for="{{navList}}" wx:key="{{index}}" >
      {{item.text}} 
      </view>
</scroll-view>
```
