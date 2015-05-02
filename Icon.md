<h1>图标</h1>
<h2>$mdIcon</h2>
<p>$mdIcon服务是一个用于查找SVG图标的函数</p>
<h3>用法</h3>
<code>
	function SomeDirective($mdIcon) {<br/>
	  // See if the icon has already been loaded, if not<br/>
	  // then lookup the icon from the registry cache, load and cache<br/>
	  // it for future requests.<br/>
	  // NOTE: ID queries require configuration with $mdIconProvider<br/>
	  $mdIcon('android').then(function(iconEl)    { element.append(iconEl); });<br/>
	  $mdIcon('work:chair').then(function(iconEl) { element.append(iconEl); });<br/>
	  // Load and cache the external SVG using a URL<br/>
	  $mdIcon('img/icons/android.svg').then(function(iconEl) {<br/>
	    element.append(iconEl);<br/>
	  });<br/>
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
