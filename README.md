# static-web-page
	仿写的一个静态页面。原网站地址为:http://www.pxwlkj.cn/

# 项目页面预览
	https://yuliangbin.github.io/static-web-page/index1.html

# 目录结构
	|-css
		|- normalize.css
		|- style1.css
	|- images
	|- index1.html
	|- README.md

# 项目总结 

# 层模型
	- fixed
		- 固定定位，相对于浏览器窗口定位
	-relative
		- 保留原来位置进行定位，即后面的元素不会挤上来(不会影响布局)。但是当进行relative定位的元素设置了top,left时，仍然会覆盖其他元素
		- 定位关系：相对于原来自己的初始位置进行定位
	
	-absolute
		- 相对于当前视口定位
		- 脱离原来位置进行定位，每一个absolute都是新的一个层。基层的元素会挤上来
		- 定位关系：相对于最近的有定位的父级元素（带有relative或fixed的元素）进行定位，如果没有的话则相对于文档进行定位。
		- 知一个父级元素的子元素设置了绝对定位的话，父元素的高度是不能自适应的(即不能随内容的增加自动撑开父级元素)。

	- 一般用relative作为参照物,absolute来进行定位。因为relative会保留原来的位置，不会造成文档的混乱，absolute是脱离原来文档进行定位，后面的元素会窜上来
	- absolute的元素相对于relative定位的属性为top bottom right left .

	- 若相对于父级元素绝对定位的元素没有设置left,right,bottom,top,则默认的有一个left:padding-left,top:padding-top.
	- 定位逻辑: 以padding的左上角为基准点),以对应的边作为参考线。向父级元素的盒子内偏移为加，往盒子外偏移为减。
		以left偏移为例
			- left=5px;距离父级元素的左padding参考线5px;即以父级元素左padding为参考线，在盒子内部向右移5px，
			- left=-5px;则以左padding为参考线向盒子外(即往左移)5px.
				
# 浮动元素float的影响
	- 所有产生了浮动流的元素我们称为浮动元素，两个属性:left为左浮动 right为右浮动。
	- 凡是设置了position:absolute,float:left\right的元素，都会在内部把元素转换成inline-block；
	- 浮动元素不会对其前面的块级元素产生影响，因为块级元素是独占一行的。
	- 会对产生了bfc的元素和文本类属性（inline）的元素以及文本产生影响。具体的影响为挤开相应元素，使相应元素向后排列。

# 浮动元素对其父级元素产生的影响: 
	由于父级元素是块级元素，所以对浮动元素来说，相当于不存在(但实际上，浮动元素还是父级元素的子级，调节父级的不透明度，子级的不透明度也会跟着变)。
	若想消除浮动子级元素对父级元素的影响，可以把父级元素调成inline-block属性(position:absolute,display:inline-block等，即设置成BFC元素)，或者清除浮动(clear:both).	

# 浮动元素对其后面的块级元素或行内元素产生的影响：
	- 浮动元素相对于块级元素来说在其上一层(只是效果上，并不是真正意义上的在其上一层，因为相对于文本类属性的元素，两者又是不影响的)，会遮盖掉块级元素(块级元素相对于浮动元素来说是不存在的),并且块级元素上的文本会在浮动元素下方出现。
	若希望消除浮动元素对后面块级元素产生的影响，可以用属性：clear:both.(只有块级元素才生效，即能清除浮动的只有块级元素)
	
	- 对其后面产生了bfc的元素或文本类属性（inline）的元素以及文本没有影响，即正常显示。
	
# 伪元素
	伪元素存在于任意一个标签里面，一个元素诞生时就已经天生的出现两个伪元素。一个在逻辑的最前面，一个在逻辑的最后面。
	注: 伪元素是一个行级元素(inline)，一定要注意伪元素是一个元素，而不是相应标签里面的属性.
	可以用伪元素来清除浮动,以 span标签为例:
		span::before{
				content:""; //content属性必须有
				display: block;
				clear: both;
			}
			
# BFC 元素
	BFC为块级格式化上下文，和普通的文档流相比，它有自己独立的一套渲染机制。
	触发BFC的四种方式:
		- float的值不是none。
		- position的值不是static或者relative。
		- display的值是inline-block、table-cell、flex、table-caption或者inline-flex
		- overflow的值不是visible
	凡是设置了position:absolute,float:left\right的元素，都会在内部把元素转换成inline-block；
	
	BFC会产生margin合并问题，但也能解决margin合并问题
	BFC能解决浮动流产生的影响:
	若希望消除浮动元素对后面块级元素产生的影响，可以对块级元素使用属性：clear:both.(只有块级元素才生效，即能清除浮动的只有块级元素)，或者对浮动元素的父级元素使用BFC或clear: both;
				
# 相邻元素的垂直方向margin合并的问题(主要是因为相邻元素在同一个BFC元素内)
	方案1：
		- 触发盒子的BFC，或在元素上加一个父级，触发父级元素的BFC。但这样增加了一层无意义的HTML标签
	方案2:
		- 不要将margin分别设在两个元素上，而是设置在一个元素上。
		
# 父子元素的垂直margin塌陷的问题
	垂直方向的margin，父子元素的移动是一起的，取最大的一个值。这是一个bug:叫margin塌陷.
	- 方案1(不建议使用)：为父级设置一个透明的边框。
		border-top:1px;solid;transparent;
    - 方案2(开发规范使用):触发一个盒子的BFC(block format context),
            - 正常情况下，每一个html元素的盒子都有一套标准的渲染规则.
			- BFC就是通过一些特殊的手段让其中的某个或几个盒子的渲染规则发生改变。即BFC设置之后，特定的盒子会遵守另一套渲染规则
            - 触发方法为：position:absolute,display:inline-block,float:left/right,overflow:hidden,
            - 盒子定位变成position:absolute后，或者display:inline-block，或者float:left/right，或者overflow:hidden。只要设置了上面属性中的一个，就触发了BFC
