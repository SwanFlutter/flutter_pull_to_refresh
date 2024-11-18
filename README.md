# flutter_pulltorefresh
<a href="https://pub.dev/packages/pull_to_refresh">
  <img src="https://img.shields.io/pub/v/pull_to_refresh.svg"/>
</a>
<a href="https://flutter.dev/">
  <img src="https://img.shields.io/badge/flutter-%3E%3D%202.0.0-green.svg"/>
</a>
<a href="https://opensource.org/licenses/MIT">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg"/>
</a>

## Intro
a widget provided to the flutter scroll component drop-down refresh and pull up load.support android and ios.
If you are Chinese,click here([中文文档](https://github.com/peng8350/flutter_pulltorefresh/blob/master/README_CN.md))



## Features
* pull up load and pull down refresh
* It's almost fit for all Scroll witgets,like GridView,ListView...
* provide global setting of default indicator and property
* provide some most common indicators
* Support Android and iOS default ScrollPhysics,the overScroll distance can be controlled,custom spring animate,damping,speed.
* horizontal and vertical refresh,support reverse ScrollView also(four direction)
* provide more refreshStyle: Behind,Follow,UnFollow,Front,provide more loadmore style
* Support twoLevel refresh,implments just like TaoBao twoLevel,Wechat TwoLevel
* enable link indicator which placing other place,just like Wechat FriendCircle refresh effect

## Usage

add this line to pubspec.yaml

```yaml

   dependencies:

    pull_to_refresh_flutter3: ^0.0.1


```

import package

```dart

    import 'package:pull_to_refresh_flutter3/pull_to_refresh_flutter3.dart';

```

simple example,***It must be noted here that ListView must be the child of SmartRefresher and cannot be separated from it. For detailed reasons, see <a href="child">here</a>***

```dart


  List<String> items = ["1", "2", "3", "4", "5", "6", "7", "8"];
  RefreshController _refreshController =
      RefreshController(initialRefresh: false);

  void _onRefresh() async{
    // monitor network fetch
    await Future.delayed(Duration(milliseconds: 1000));
    // if failed,use refreshFailed()
    _refreshController.refreshCompleted();
  }

  void _onLoading() async{
    // monitor network fetch
    await Future.delayed(Duration(milliseconds: 1000));
    // if failed,use loadFailed(),if no data return,use LoadNodata()
    items.add((items.length+1).toString());
    if(mounted)
    setState(() {

    });
    _refreshController.loadComplete();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SmartRefresher(
        enablePullDown: true,
        enablePullUp: true,
        header: WaterDropHeader(),
        footer: CustomFooter(
          builder: (BuildContext context,LoadStatus mode){
            Widget body ;
            if(mode==LoadStatus.idle){
              body =  Text("pull up load");
            }
            else if(mode==LoadStatus.loading){
              body =  CupertinoActivityIndicator();
            }
            else if(mode == LoadStatus.failed){
              body = Text("Load Failed!Click retry!");
            }
            else if(mode == LoadStatus.canLoading){
                body = Text("release to load more");
            }
            else{
              body = Text("No more Data");
            }
            return Container(
              height: 55.0,
              child: Center(child:body),
            );
          },
        ),
        controller: _refreshController,
        onRefresh: _onRefresh,
        onLoading: _onLoading,
        child: ListView.builder(
          itemBuilder: (c, i) => Card(child: Center(child: Text(items[i]))),
          itemExtent: 100.0,
          itemCount: items.length,
        ),
      ),
    );
  }

  // from 1.5.0, it is not necessary to add this line
  //@override
 // void dispose() {
    // TODO: implement dispose
  //  _refreshController.dispose();
  //  super.dispose();
 // }

```

The global configuration RefreshConfiguration, which configures all Smart Refresher representations under the subtree, is generally stored at the root of MaterialApp and is similar in usage to ScrollConfiguration.
In addition, if one of your SmartRefresher behaves differently from the rest of the world, you can use RefreshConfiguration.copyAncestor() to copy attributes from your ancestor RefreshConfiguration and replace
attributes that are not empty.

```dart
    // Smart Refresher under the global configuration subtree, here are a few particularly important attributes
     RefreshConfiguration(
         headerBuilder: () => WaterDropHeader(),        // Configure the default header indicator. If you have the same header indicator for each page, you need to set this
         footerBuilder:  () => ClassicFooter(),        // Configure default bottom indicator
         headerTriggerDistance: 80.0,        // header trigger refresh trigger distance
         springDescription:SpringDescription(stiffness: 170, damping: 16, mass: 1.9),         // custom spring back animate,the props meaning see the flutter api
         maxOverScrollExtent :100, //The maximum dragging range of the head. Set this property if a rush out of the view area occurs
         maxUnderScrollExtent:0, // Maximum dragging range at the bottom
         enableScrollWhenRefreshCompleted: true, //This property is incompatible with PageView and TabBarView. If you need TabBarView to slide left and right, you need to set it to true.
         enableLoadingWhenFailed : true, //In the case of load failure, users can still trigger more loads by gesture pull-up.
         hideFooterWhenNotFull: false, // Disable pull-up to load more functionality when Viewport is less than one screen
         enableBallisticLoad: true, // trigger load more by BallisticScrollActivity
        child: MaterialApp(
            ........
        )
    );

```

1.5.6 add new feather: localization ,you can add following code in MaterialApp or CupertinoApp:

```dart

    MaterialApp(
            localizationsDelegates: [
              // this line is important
              RefreshLocalizations.delegate,
              GlobalWidgetsLocalizations.delegate,
              GlobalMaterialLocalizations.delegate
            ],
            supportedLocales: [
              const Locale('en'),
              const Locale('zh'),
            ],
            localeResolutionCallback:
                (Locale locale, Iterable<Locale> supportedLocales) {
              //print("change language");
              return locale;
            },
    )

```

## ScreenShots

### Examples

<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/c183ca8f-a67b-4b27-b472-169c203111b3" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/274f2120-fa65-4248-bd16-c0dc15fb8be6" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/477e962b-f7ac-4012-be2f-50c22e76265e" width="200"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/d3589b48-0f50-4880-99f3-53ea535838c3" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/a84e70fc-770e-47c6-805b-a621cc8ad7d3" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/f8e6ea94-9b25-48c4-82ec-5e3cd29dc98d" width="200"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/d8400aa4-0c9f-4784-809f-46683bb2fc25" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/8dbf0f56-40da-41cd-b9b8-f54c1ec3816c" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/3a9e22c0-8fcf-4838-9a3d-b10d8e4239d6" width="200"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/1db2e1ea-b834-4616-8743-36ef5e748e56" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/4c0c6114-c34f-460e-9a8b-dba85ca26551" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/0bca74ad-f81f-446e-a245-c3ef85bd823a" width="200"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/3d8933d9-6cf2-40d8-9f5f-9f04bb5c311d" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/b5e0108c-39db-4aa3-b888-2415eaa95c22" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/1e4d8a66-6fba-41c0-adfb-400f0279946d" width="200"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/363a1ce4-f452-4575-909a-f36230318723" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/280c6b93-bf38-4fc2-b5ff-bdfd2580cdeb" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/c10490ab-23bd-49de-91bc-959c1d89b7a7" width="200"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/5bd17159-a8d1-4e3a-b26f-ca12256f3278" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/934e035d-8d32-4f0c-9b25-71bbaed5c6dd" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/1266ace5-1173-4db8-adea-aae98e4e4865" width="200"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/88cd94a8-243f-47c2-82a7-eb256e4fdd41" width="200"></td>
    <td><img src="https://github.com/user-attachments/assets/b76918c1-7bd3-4fb9-b3af-d4f7bbe39f25" width="200"></td>
  </tr>
</table>

### 各种指示器

| refresh style |   |pull up load style| |
|:---:|:---:|:---:|:---:|
| RefreshStyle.Follow <br>![Follow](example/images/refreshstyle1.gif)|RefreshStyle.UnFollow <br> ![不跟随](example/images/refreshstyle2.gif)| LoadStyle.ShowAlways <br>  ![永远显示](example/images/loadstyle1.gif) | LoadStyle.HideAlways<br> ![永远隐藏](example/images/loadstyle2.gif)|
| RefreshStyle.Behind <br> ![背部](example/images/refreshstyle3.gif)| RefreshStyle.Front <br> ![前面悬浮](example/images/refreshstyle4.gif)| LoadStyle.ShowWhenLoading<br>  ![当加载中才显示,其它隐藏](example/images/loadstyle3.gif) | |

|Style| [ClassicIndicator](https://github.com/peng8350/flutter_pulltorefresh/blob/master/lib/src/indicator/classic_indicator.dart) | [WaterDropHeader](https://github.com/peng8350/flutter_pulltorefresh/blob/master/lib/src/indicator/waterdrop_header.dart) | [MaterialClassicHeader](https://github.com/peng8350/flutter_pulltorefresh/blob/master/lib/src/indicator/material_indicator.dart) |
|:---:|:---:|:---:|:---:|
|| ![](example/images/classical_follow.gif) | ![](example/images/warterdrop.gif) | ![](example/images/material_classic.gif) |

|Style|  [WaterDropMaterialHeader](https://github.com/peng8350/flutter_pulltorefresh/blob/master/lib/src/indicator/material_indicator.dart) | [Shimmer Indicator](example/lib/ui/example/customindicator/shimmer_indicator.dart) |[Bezier+Circle](https://github.com/peng8350/flutter_pulltorefresh/blob/master/lib/src/indicator/bezier_indicator.dart) |
|:---:|:---:|:---:|:---:|
||  ![](example/images/material_waterdrop.gif) |![](example/images/shimmerindicator.gif) | ![](example/images/bezier.gif) |


<a name="child"></a>

## about SmartRefresher's child explain

Since 1.4.3, the child attribute has changed from ScrollView to Widget, but this does not mean that all widgets are processed the same. SmartRefresher's internal implementation mechanism is not like  NestedScrollView<br><br>
There are two main types of processing mechanisms here, `the first category`is the component inherited from ScrollView. At present, there are only three types,
 `ListView`, `GridView`, `CustomScrollView`. ` The second category ` is components that are not inherited from ScrollView, which generally put empty views,
  NoScrollable views (NoScrollable convert Scrollable), PageView, and you don't need to estimate height  by `LayoutBuilder` yourself.
<br><br>
For the first type of mechanism, slivers are taken out of the system "illegally". The second is to put children directly into classes such as `SliverToBox Adapter'. By splicing headers and footers back and forth to form slivers, and then putting slivers inside Smart Refresher into CustomScrollView, you can understand Smart Refresher as CustomScrollView,
because the inside is to return to CustomScrollView. So, there's a big difference between a child node and a ScrollView.
<br><br>
Now, guess you have a requirement: you need to add background, scrollbars or something outside ScrollView. Here's a demonstration of errors and correct practices

```dart

   //error
   SmartRefresher(
      child: ScrollBar(
          child: ListView(
             ....
      )
    )
   )

   // right
   ScrollBar(
      child: SmartRefresher(
          child: ListView(
             ....
      )
    )
   )

```

Demonstrate another wrong doing,put ScrollView in another widget

```dart

   //error
   SmartRefresher(
      child:MainView()
   )

   class MainView extends StatelessWidget{
       Widget build(){
          return ListView(
             ....
          );
       }

   }

```

The above mistake led to scrollable nesting another scrollable, causing you to not see the header and footer no matter how slippery you are.
Similarly, you may need to work with components like NotificationListener, ScrollConfiguration..., remember, don't store them outside ScrollView (you want to add refresh parts) and Smart Refresher memory.。


## More
- [Property Document](propertys_en.md) or [Api/Doc](https://pub.dev/documentation/pull_to_refresh/latest/pulltorefresh/SmartRefresher-class.html)
- [Custom Indicator](custom_indicator_en.md)
- [Inner Attribute Of Indicators](indicator_attribute_en.md)
- [Update Log](CHANGELOG.md)
- [Notice](notice_en.md)
- [FAQ](problems_en.md)


## Exist Problems
* about NestedScrollView,When you slide down and then slide up quickly, it will return back. The main reason is that
 NestedScrollView does not consider the problem of cross-border elasticity under
 bouncingScrollPhysics. Relevant flutter issues: 34316, 33367, 29264. This problem
 can only wait for flutter to fix this.
* SmartRefresher does not have refresh injection into ScrollView under the subtree, that is, if you put AnimatedList or RecordableListView in the child
 is impossible. I have tried many ways to solve this problem and failed. Because of the
 principle of implementation, I have to append it to the head and tail of slivers. In fact, the problem is not that much of my
Component issues, such as AnimatedList, can't be used with AnimatedList and GridView unless
 I convert AnimatedList to SliverAnimatedList is the solution. At the moment,
 I have a temporary solution to this problem, but it's a bit cumbersome to rewrite the code inside it and then outside ScrollView.
Add SmartRefresher, see my two examples [Example 1](example/lib/other/refresh_animatedlist.dart)和[Example 2](example/lib/other/refresh_recordable_listview.dart)

## Thanks

[SmartRefreshLayout](https://github.com/scwang90/SmartRefreshLayout)
