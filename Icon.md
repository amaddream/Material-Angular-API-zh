<h1>图标</h1>
<h2>$mdIcon</h2>
<p>$mdIcon服务是一个用于查找SVG图标的函数</p>
<h4>用法</h4>
<code>
````
	function SomeDirective($mdIcon) {
	//查看图标是否已被加载，如果没有
	//则从注册表缓存查找图标，然后加载并缓存
	//以备将来的请求
	//注意：ID查询需要以下配置
		$mdIconProvider 
		$mdIcon('android').then(function(iconEl)    { element.append(iconEl); }); 
		$mdIcon('work:chair').then(function(iconEl) { element.append(iconEl); }); 
		//用一个URL加载和缓存外部SVG
		$mdIcon('img/icons/android.svg').then(function(iconEl) { 
			element.append(iconEl); 
		});
	};
````
</code>

<p>
	<strong>注意:</strong>md-icon指令在内部调用$mdIcon服务查询、加载和实例化SVG DOM元素。
</p>
<h3>参数</h3>
<table>
	<tr>
		<th>参数</th>
		<th>类型</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>*id</td>
		<td>字符串</td>
		<td>惟一id或URL的查询值。如果参数是URL，服务将从它的内部缓存中检索该图标元素，或先加载图标并缓存，再执行此操作。如果该值不是URL类型的字符串，则将执行一次ID查询。id可以是一个惟一的图标ID或者是包含一个图标集ID前缀的字符串。<br><br>
		要使id查询正确地运行，意味着必须事先用$mdIconProvider来配置所有id到URL的映射。
		</td>
	</tr>
</table>
<table>
	<tr>
		<th>返回</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>obj</td>
		<td>由SVG数据文件中的SVG标记创建的初始SVG DOM元素的副本。</td>
	</tr>
</table>  

<h2>$mdIconProvider</h2>
<p>
$mdIconProvider只用于将URL注册为ID。这些配置特性允许图标和图标集在"&lt;md-icon/&gt;"指令编译前预先注册并连接源URL。
</p>
<p>
实际的svg文件加载将延迟至按需发送的请求，并由$mdIcon服务使用$http服务内部加载。当以name/ID请求一个SVG时，$mdIcon服务将搜索相关的源URL注册表；该URL用来按需加载并动态解析SVG。
</p>
<h4>js</h4>
<code>
````
	app.config(function($mdIconProvider) {
		//用指明 [set:]id 的方式来为图标配置URL。
		$mdIconProvider
			.defaultIconSet('my/app/icons.svg')       //   注册一个默认SVG图标集
			.iconSet('social', 'my/app/social.svg')   //   为一个SVG图标集注册一个命名
			.icon('android', 'my/app/android.svg')    //   用name注册一个特定的图标
			.icon('work:chair', 'my/app/chair.svg');  //   在指定的图标集内注册图标
	});
````
</code>
<p>SVG图标和图标集可通过使用(a)构建过程 或 (b)运行时的启动过程，被轻松预加载和缓存（如下所示）：</p>
<h4>js</h4>
<code>
````
app.config(function($mdIconProvider) {
	//注册一套默认的SVG图标定义  
	$mdIconProvider.defaultIconSet('my/app/icons.svg')
})
.run(function($http, $templateCache){
	// 用URL和缓存从$templateCache预先获取图标源...
	// 随后$http调用会先从此处查看
	var urls = [ 'imy/app/icons.svg', 'img/icons/android.svg'];
	angular.forEach(urls, function(url) {
		$http.get(url, {cache: $templateCache});
	});
});
````
</code>
<P><strong>注意：</strong>SVG数据随后被内部缓存起来以备之后的请求</P>

<h3>用法</h3>
<p>$mdIconProvider();</p>

<h3>方法</h3>
<h4>$mdIconProvider.icon(id,url,[iconsize]);</h4>
<p>为指定的图标名注册一个源URL；命名可以包含可选的“图标集”命名前缀。这些图标之后将用$mdIcon(&lt;icon name&gt;)从缓存检索。</p>

<table>
	<tr>
		<th>参数</th>
		<th>类型</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>*id</td>
		<td>string</td>
		<td>用于注册图标的图标name/id</td>
	</tr>
	<tr>
		<td>*url</td>
		<td>string</td>
		<td>指定数据文件的外部位置。被用来内部使用$http加载数据或如果配置了预加载的话，作为在$templateCache内查找的一部分。</td>
	</tr>
	<tr>
		<td>iconSize</td>
		<td>string</td>
		<td>用于指定集合内图标宽高的数字。图标集内的所有图标必须保持相同的尺寸。默认尺寸是24.</td>
	</tr>
</table>
<table>
	<tr>
		<th>返回</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>obj</td>
		<td>一个对$mdIconProvider的引用；用于支持链式方法调用的API</td>
	</tr>
</table>
<h4>$mdIconProvider.iconSet(id,url, [iconSize]);</h4>
<p>为“指定的”图标集合注册一个源URL。每个定义都有一个图标ID的SVG组定义。独立的图标随后可以通过$mdIcon(&lt;icon set name&gt;:&lt;icon name&gt;)从缓存中被检索到。</p>
<table>
	<tr>
		<th>参数</th>
		<th>类型</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>*id</td>
		<td>string</td>
		<td>用来注册图标集的图标name/id</td>
	</tr>
	<tr>
		<td>*url</td>
		<td>string</td>
		<td>指定数据文件的外部位置。被用来内部使用$http加载数据或如果配置了预加载的话，作为在$templateCache内查找的一部分。</td>
	</tr>
	<tr>
		<td>iconSize</td>
		<td>string</td>
		<td>用于指定集合内图标宽高的数字。图标集内的所有图标必须保持相同的尺寸。默认尺寸是24.</td>
	</tr>
</table>
<h4>$mdIconProvider.defaultIconSet(url, [iconSize]);</h4>
<p>为默认“指定的”图标集注册一个源URL。除非显式注册，否则对图标的查找将转移至“默认的”图标集。可通过$mdIcon(&lt;icon name&gt;)检索到这个被缓存的，默认的图标集。</p>
<table>
	<tr>
		<th>参数</th>
		<th>类型</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>*url</td>
		<td>string</td>
		<td>指定数据文件的外部位置。被用来内部使用$http加载数据或如果配置了预加载的话，作为在$templateCache内查找的一部分。</td>
	</tr>
	<tr>
		<td>iconSize</td>
		<td>string</td>
		<td>用于指定集合内图标宽高的数字。图标集内的所有图标必须保持相同的尺寸。默认尺寸是24.</td>
	</tr>
</table>
<h4>$mdIconProvider.defaultIconSize(iconSize);</h4>
<p>&lt;md-icon/&gt;标记也可以用CSS来指定样式，同时可以用这个方法来配置所有图标默认宽度和高度；除非你指定了CSS进行覆盖。默认尺寸是(24px,24px).</p>
<table>
	<tr>
		<th>参数</th>
		<th>类型</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>*iconSize</td>
		<td>string</td>
		<td>用于指定图标集中图标宽度和高度的数字。所有图标集中的图标必须保持相同的尺寸。默认尺寸为24</td>
	</tr>
</table>
<table>
	<tr>
		<th>返回</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>obj</td>
		<td>一个$mdIconProvider引用，用于支持链式方法调用的API</td>
	</tr>
</table>
<h2>md-icon</h2>
<p>md-icon指令是用于展示一个字体图标或者SVG图标的标记元素。不管是外部SVG（通过URL）还是来自图标集的缓存SVG都能很容易地被加载和使用。</p>

<h3>用法</h3>
<h4>html</h4>
<code>
````
	<md-icon md-font-icon="android" alt="android "></md-icon>  
	<md-icon md-svg-icon="action:android" alt="android "></md-icon>  
	<md-icon md-svg-src="/android.svg" alt="android "></md-icon>  
	<md-icon md-svg-src="{{ getAndroid() }}" alt="android "></md-icon>
````
</code>

<h3>属性</h3>
<table>
	<tr>
		<th>属性</th>
		<th>类型</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>*md-svg-src</td>
		<td>string</td>
		<td>用来加载、缓存和显示一个外部SVG的字符串URL[或者表达式]</td>
	</tr>
	<tr>
		<td>*md-svg-icon</td>
		<td>string</td>
		<td>用来从内部缓存查询图标的字符串name；插值字符串或表达式都可以用。可以用语法&lt;set name\&gt;:&lt;icon name\&gt;来使用特定的集合名。  

		为了使用图标集（icon sets），开发者需要用$mdIconProvider服务预先注册这个图标集。
		</td>
	</tr>
	<tr>
		<td>*md-font-icon</td>
		<td>string</td>
		<td>CSS图标关联字体的字符串name将被用于渲染图标。需要预先加载字体和指定的CSS样式。</td>
	</tr>
	<tr>
		<td>alt</td>
		<td>string</td>
		<td>图标的可访问标注。如果提供了空字符串，图标将以aria-hidden="true"的方式从访问层隐藏。如果图标的父元素没有alt也没有标注，控制台将记录一个警告。</td>
	</tr>
</table>