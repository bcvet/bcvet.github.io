#bnsy-gird使用文档

## 结构

- 浏览器兼容代码
- 布局代码
- 常用基础class（例：.bn-clearfix, .bn-ellipsis ）

## 使用手册

### 清除浮动

- **清除浮动使用`.bn-clearfix`,在浮动元素的父级添加bn-cleafix**

使用示例：
	
	<style>
		.nav{
			float:left;
			height:100px;
		}
		.main{
			float:right;
			height:100px;
		}
	<style>
	
	<div class="bn-clearfix">
		<div class="nav">...</div>
		<div class="main">...</div>
	</div>	
	
### 超长显示省略号

- 使用`.bn-ellipsis`，注意：只针对单行文字超出才生效！并且需规定当前元素的宽度。

### 布局

- 块级元素居中显示。使用`.bn-container-center`，对于其它的块元素，你需要设定一个宽度。
- 向一个父元素添加 .bn-grid 类来创建网格容器。对子元素添加一个 `.bn-gird-*` 类来限定单元格的宽度。网格支持1、2、3、4、5、6和12个单元划分。下面的列表为你提供了可以应用到单元的 `.bn-gird-*` 类的概述。

<table style="width:100%">
	<tr>
		<th>class</th>
		<th>描述</th>
	<tr>
	<tr>
		<td> .bn-gird-1-1 </td>
		<td>填满可见宽度的100％。</td>
	</tr>
	<tr>
		<td> .bn-gird-1-2 </td>
		<td> 把网格等分为两半。</td>
	</tr>
	<tr>
		<td> .bn-gird-1-3 to .bn-gird-2-3 </td>
		<td> 将网格划分成三分之一。</td>
	</tr>
	<tr>
		<td> .bn-gird-1-4 to .bn-gird-3-4 </td>
		<td> 将网格划分成四分之一。</td>
	</tr>
	<tr>
		<td> .bn-gird-1-5 to .bn-gird-4-5</td>
		<td> 将网格划分成五分之一。</td>
	</tr>
	<tr>
		<td> .bn-gird-1-6to .bn-gird-5-6 </td>
		<td> 将网格划分成六分之一。</td>
	</tr>
	<tr>
		<td> .bn-gird-1-12to .bn-gird-11-12 </td>
		<td> 将网格划分成十二分之一。</td>
	</tr>
</table>

使用示例：

	<div class="bn-grid">
		<div class="bn-gird-1-2">...</div>
		<div class="bn-gird-1-2">...</div>
	</div>

>注：bn-gird布局默认为无间距，如需要添加间距，可以使用`.bn-gutter-small` `.bn-gutter-middle` `.bn-gutter-large` 分局对应间距的小、中、大
>把左侧第一个元素距离左侧距离去掉 需添加.bn-gird-no-margin-left。

使用示例：

	<div class="bn-gird bn-gutter-small">
		<div class='bn-gird-1-2 bn-gird-no-margin-left'></div>		<div class='bn-gird-1-2'></div>
	</div>
	
- 子元素均分

使用示例：

	<div class="bn-gird bn-gird-width-1-3">
		<div>均分第一块</div>
		<div>均分第二块</div>
		<div>均分第三块</div>
	</div>
	
>**可选class为`.bn-gird-width-1-2` `.bn-gird-width-1-3` `.bn-gird-width-1-4` `.bn-gird-width-1-5` `.bn-gird-width-1-6` `.bn-gird-width-1-12`**

### 元素之间的间距
- 各块级元素都能使用，间距分5px, 10px，15px，20px 三个等级
对应的class名称为属性缩写单词加对应像素，例如：

距离左侧15像素，属性为margin-left:15px； 对应的class为.bn-ml15 </br>
距离右侧10像素，属性为margin-left:15px； 对应的class为.bn-mr10

- 元素内距离使用padding，例如

距离左侧15像素，属性为padding-left:15px； 对应的class为.bn-pl15
</br>
距离右侧10像素，属性为padding-left:15px； 对应的class为.bn-pr10

### 按钮
按钮统一引用.bn-btn 样式，即为默认样式

使用示例：

	<button class="bn-btn"></button>
	<input type="button" class="bn-btn" />
	<a class="bn-btn" href="#"></a>

按钮的颜色可改变，需引用bn-btn-blue，只需添加对应颜色即可！

使用示例：

	<button class="bn-btn bn-btn-blue"></button>

按钮的大小可改变，需引用.bn-btn-mini，.bn-btn-small，.bn-btn-large

使用示例：

	<button class="bn-btn bn-btn-mini"></button>
	
- 按钮组

创建一个按钮组，在包裹着按钮们的 <div> 元素中添加 .bn-btn-group 类即可。

使用示例：

	<div class="bn-btn-group">
    		<a class="bn-btn" href="">...</a>
    		<button class="bn-btn">...</button>
    		<button class="bn-btn">...</button>
	</div>
	
### 导航菜单

使用这个组件，添加 .bn-nav 类到一个 `<ul>` 元素中。使用 `<a>` 元素作为列表中的菜单项。要对一个菜单项应用选中状态的效果，添加 .bn-cur 类即可。

默认导航为纵向排列，如果需要横向需要添加`.bn-direction-row`。<br>
<br>
如果需要间隔线，需要`.bn-nav-space`。
<br>
使用示例：

	<ul class="bn-nav bn-direction-row bn-nav-space ">
		<li class="bn-cur"><a href="#">个人中心六字</a></li>
		<li><a href="#">课程中心</a></li>
		<li><a href="#">课例中心</a></li>
		<li><a href="#">资源</a></li>
	<ul>
 
 带图标的竖向导航
 
 使用示例：
 
 	
 		<ul class="bn-nav">
 			<li class="bn-cur">
 				<a href="">
 					<i class="iconfont icon-person-center"></i>
 					中心首页
				</a>
			</li>
		</ul>


 
### 个人面板
- 个人面板包含人员头像，人员姓名以及下拉符号！并且可下拉。

使用示例：

	<div class="bn-panel">
		<div class="bn-panel-box">
			<img src="../bcvet/images/common2/test-01.jpg" alt="">
			<span>张三丰</span>
			<i class="iconfont icon-dropdown1"></i>
		</div>
		<div class="bn-panel-drop-box">
		 	<ul>
		 		<li><a href="#">安全退出</a></li>
		 		<li><a href="#">个人设置</a></li>
	 		</ul>
 		</div>
	 </div>

>.bn-panel 为个人面板外容器，.bn-panel-box是当前显示人员头像、姓名、下拉图标容器， .bn-panel-drop-box 为下拉列表的外容器。 **注意：默认.bn-panel是没有宽度的，宽度视使用环境自定义，如果元素浮动的话，则宽度可省略，默认为最小宽度**	 

### 列表
-  左侧标题  右侧更多类似左右形式 ，需要.bn-title。
</br>
(标题--更多) （切换--更多）（切换--删除）

标题使用示例：

	<div class="bn-title">
		<h4 class="bn-fl">最新公告</h4>
		 <a href="#" class="bn-more">更多>></a>
	</div>

> .bn-title 有默认的边距，默认为padding: 20px 20px 10px 20px; 如果去掉，需使用.bn-no-padding。.bn-title默认为没有底边线，如果添加，需使用.bn-bottom-border。


> 标题默认为黑色，如需出现左侧边线需使用.bn-border-left 。


切换使用示例:

	<div class="bn-title bn-bottom-border">
		<ul class="bn-tab bn-clearfix">
			 <li class="bn-cur">
			 	<a href="">我关注谁</a>
		 	</li>
		 	<li>|</li>
		 	<li>
		 		<a href="">谁关注我</a>
	 		</li>
 		</ul>
	</div>

> 注意: `<li>|</li>` 为必须书写代码，请勿删除

- 内容列表，左侧内容，右侧（日期、姓名等等）

使用示例：

	<div class="bn-list-row">
		 <ul>
		 	<li>
		 		<p>2015年“国培计划”新疆中小学幼儿园信息技术应用能力提升工程</p>
		 		<span>2016-2-16</span>
	 		</li>
	 		<li>
	 		<p>2015年“国培计划”新疆中小学幼儿园信息技术应用能力提升工程</p>
	 		<span>2016-2-16</span>
 			</li>
		</ul>
	</div>

### 翻页

使用示例:

	<div class="bn-pagination">
		<ul>
			<li>
				<a href="" title="第一页">
					<span class="iconfont icon-first"></span>
				</a>
			</li>
			<li>
				<a href="">
					<span class="iconfont icon-prevpage"></span>
				</a>
			</li>
			<li>
				<input type="text" class="bn-page-input">
				<span class="bn-page-text">共2页</span>
			</li>
			<li>
				<a href="">
					<span class="iconfont icon-nextpage"></span>
				</a>
			</li>
			<li>
				<a href="" title="最后一页">
					<span class="iconfont icon-last"></span>
				</a>
			</li>
		 </ul>
	  </div>


- 前台假分页

使用示例：
	
	<ul class="bn-switch bn-clearfix">
		<li>
			<a href="" class="iconfont icon-aleft"></a>
		 </li>
		 <li class="bn-cur">
		 	<a href="" class="iconfont icon-right"></a>
	 	</li>
 	</ul>

### 访客列表

使用示例: 

	<div class="bn-photo-box bn-visitor">
	
		<!--tab切换（此处用的是切换形式的.bn-title使用）-->
		<div class="bn-title bn-bottom-border">
			<ul class="bn-tab bn-clearfix">
				<li class="bn-cur">
					<a href="">我关注谁</a>
				</li>
				<li>|</li>
				<li>
					<a href="">谁关注我</a>
				</li>
			</ul>
		</div>
		
		<!--标题（此处用的是bn-title组件）-->
		<div class="bn-title bn-bottom-border bn-clearfix">
			<h4 class="bn-fl">最近访客</h4>
			<p class="bn-fr">总浏览数10000</p>
		</div>
		
		<!--正文-->
		<div class="bn-visitors">
		 <ul class="bn-visitors-box bn-clearfix">
		 	<li>
		 		<a href="">
		 			<img src="../bcvet/images/common2/test-01.jpg" title="" alt="" />
	 				<p class="bn-visitor-name">Angela</p>
	 				<p class="bn--visitor-time">12月30日</p>
	 			</a>
 			</li>
 			<li>
 				<a href="">
 					<img src="../bcvet/images/common2/test-01.jpg" title="" alt="" />
 					<p class="bn-visitor-name">范冰冰</p>
 					<p class="bn-visitor-time">1月5日</p>
				</a>
			</li>
			<li>
				<a href="">
					<img src="../bcvet/images/common2/test-01.jpg" title="" alt="" />
					<p class="bn-visitor-name">阮一峰</p>
					<p class="bn-visitor-time">1月5日</p>
				</a>
			</li>
		</ul>
		</div>
		
		<!-- 切换 tab （即前台假分页） -->
		 <ul class="bn-switch  bn-clearfix">
		 	<li>
		 		<a href="" class="iconfont icon-aleft"></a>
	 		</li>
	 		<li class="bn-cur">
	 			<a href="" class="iconfont icon-right"></a>
 			</li>
		</ul>
		
	</div>
	

### 面包屑

使用示例：

	<ul class="bn-breadcrumb">
		<li><a href="">一级</a></li>
		<li><a href="">二级</a></li>
		<li><span>禁用</span></li>
		<li class="bn-active"><span>当前</span></li>
	</ul>	
	
	
### 学科学段筛选条件

使用示例 : 

	<div class="bn-filter">
            <span>学段：</span>
            <ul class="bn-filter-row-2">
                <li class="bn-cur"><a href="">全部</a></li>
                <li><a href="">幼儿教育</a></li>
                <li><a href="">小学</a></li>
                <li><a href="">初中</a></li>
                <li><a href="">高中</a></li>
                <li><a href="">全部</a></li>
                <li><a href="">幼儿教育</a></li>
                <li><a href="">小学</a></li>
                <li><a href="">初中</a></li>
                <li><a href="">高中</a></li>
                <li><a href="">全部</a></li>
                <li><a href="">幼儿教育</a></li>
                <li><a href="">小学</a></li>
                <li><a href="">初中</a></li>
                <li><a href="">高中</a></li>
                <li><a href="">全部</a></li>
                <li><a href="">幼儿教育</a></li>
                <li><a href="">小学</a></li>
                <li><a href="">初中</a></li>
                <li><a href="">高中</a></li>
            </ul>
            <a href="" class="bn-change-btn bn-close-btn">更多</a>
        </div>

> 筛选条件分1-3行，ul默认为单行筛选，如需2行或者3行筛选，需引用`.bn-filter-row-2 `或者 `.bn-filter-row-3`。

### 搜索框

使用示例：
	
	<div class="bn-search">
        <input placeholder="请输入搜索条件...." type="text">
    </div>
    
    
