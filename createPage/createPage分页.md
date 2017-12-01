##createPage 分页跳页问题
###demo
	html
	<div class='tcdPageCode'></div>

	javascript
	<script src="resource/module/jquery.page.js"></script>
	$(".tcdPageCode").createPage({
		pageCount:self.maxpage,//总页数
		current:self.page,//当前页码
		backFn:function(p){
			self.page = p;//返回页码
			initData();
		}
	});

	css
	.tcdPageCode {
		position: absolute;
		bottom:20px;
		left:50%;
		transform: translate(-50%);
		text-align: left;
		color: #ccc;
		text-align:center;
	}
	.tcdPageCode a {
		display: inline-block;
		color: #428bca;
		display: inline-block;
		height: 25px;	
		line-height: 25px;	
		padding: 0 10px;
		border: 1px solid #ddd;	
		margin: 0 2px;
		border-radius: 4px;
		vertical-align: middle;
	}
	.tcdPageCode a:hover {
		text-decoration: none;
		border: 1px solid #428bca;
	}
	.tcdPageCode span.current {
		display: inline-block;
		height: 25px;
		line-height: 25px;
		padding: 0 10px;
		margin: 0 2px;
		color: #fff;
		background-color: #428bca;	
		border: 1px solid #428bca;
		border-radius: 4px;
		vertical-align: middle;
	}
	.tcdPageCode span.disabled {	
		display: inline-block;
		height: 25px;
		line-height: 25px;
		padding: 0 10px;
		margin: 0 2px;	
		color: #bfbfbf;
		background: #f2f2f2;
		border: 1px solid #bfbfbf;
		border-radius: 4px;
		vertical-align: middle;
	}

- 现象：
 
   - 点击页数跳转的时候一切正常，但是点击“上一页”或者“下一页”的时候会跳两页，再请求继续累加。

- 原因：

   - 因为采用的是ajax动态获取每一页的数据，每动态生成一次数据，就会多跳转一页，如此累加。

- 解决办法：

   - 把分页`<div class='tcdPageCode'></div>`在js中生成,不要直接在jsp中写出,分页初始化时,先remove掉原来的div,然后再重新生成这个div ，再进行createPage。

###修改后 demo

	$(".tcdPageCode").remove();
	var pageHtml = "<div class='tcdPageCode'></div>";
	$(".cont").append(pageHtml);
	$(".tcdPageCode").createPage({
		pageCount:self.maxpage,
		current:self.page,
		backFn:function(p){
			self.page = p;
			initData();
		}
	});