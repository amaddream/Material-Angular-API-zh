<h1>图标</h1>
<h2>$mdIcon</h2>
<p>$mdIcon服务是一个用于查找SVG图标的函数</p>
<h4>用法</h4>
<code>
	function SomeDirective($mdIcon) {  

	  // See if the icon has already been loaded, if not  

	  // then lookup the icon from the registry cache, load and cache  

	  // it for future requests.  

	  // NOTE: ID queries require configuration with $mdIconProvider  

	  $mdIcon('android').then(function(iconEl)    { element.append(iconEl); });  

	  $mdIcon('work:chair').then(function(iconEl) { element.append(iconEl); });  

	  // Load and cache the external SVG using a URL  

	  $mdIcon('img/icons/android.svg').then(function(iconEl) {  

	    element.append(iconEl);  

	  });  

	};
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
		<td>id或者URL的惟一查询值。如果参数是URL，服务将从内部缓存检索图标元素或者首先加载图标并缓存。如果值不是URL类型字符串，则将执行一次ID查询。id可以是一个惟一的图标ID或者包含一个图标集ID的前缀。<br><br>
		要使id查询正确地运行，意味着必须事先用$mdIconProvider来配置所有id到URL映射。
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
$mdIconProvider只用于将URL注册为ID。这些配置特性允许图标和图标集在"<md-icon/>"指令编译前预先注册并连接源URL。
</p>
<p>
实际的svg文件加载将延迟至按需请求，并由$mdIcon服务使用$http服务内部加载。当以name/ID请求一个SVG时，$mdIcon服务将搜索相关的源URL的注册表；该URL用来按需加载并动态解析SVG。
</p>
<h4>js</h4>
<code>
app.config(function($mdIconProvider) {  
  // Configure URLs for icons specified by [set:]id.  
  $mdIconProvider  
       .defaultIconSet('my/app/icons.svg')       //   Register a default set of SVG icons
       .iconSet('social', 'my/app/social.svg')   //   Register a named icon set of SVGs
       .icon('android', 'my/app/android.svg')    //   Register a specific icon (by name)
       .icon('work:chair', 'my/app/chair.svg');  //   Register icon in a specific set  
});
</code>
<p>SVG图标和图标集可通过使用(a)构建过程 或 (b)运行时的启动过程，被轻松预加载和缓存（如下所示）：</p>
<h4>js</h4>
<code>
	app.config(function($mdIconProvider) {  
	// Register a default set of SVG icon definitions  
	$mdIconProvider.defaultIconSet('my/app/icons.svg')  
})  
.run(function($http, $templateCache){  
	// Pre-fetch icons sources by URL and cache in the $templateCache...  
  // subsequent $http calls will look there first.  
  var urls = [ 'imy/app/icons.svg', 'img/icons/android.svg'];  
  angular.forEach(urls, function(url) {  
    $http.get(url, {cache: $templateCache});  
  });  
});
</code>
<P><strong>注意：</strong>SVG数据随后被内部缓存起来以备之后的请求</P>

<h3>用法</h3>
<p>$mdIconProvider();</p>

<h3>方法</h3>
<p>$mdIconProvider.icon(id,url,[iconsize]);</p>