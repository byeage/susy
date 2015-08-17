###SUSY的全局变量

        $susy: (
          flow: ltr,
          math: fluid,
          output: float,
          gutter-position: after,
          container: auto,
          container-position: center,
          columns: 4,
          gutters: .25,
          column-width: false,
          global-box-sizing: content-box,
          last-flow: to,
          debug: (
            image: hide,
            color: rgba(#66f, .25),
            output: background,
            toggle: top right,
          ),
          use-custom: (
            background-image: true,
            background-options: false,
            box-sizing: true,
            clearfix: false,
            rem: true,
          )
        );
        
###你可以定制自己的全局变量，或者根据需求创建布局map
		$susy: (
          columns: 12,
          gutters: .25,
          gutter-position: inside,
        )
  
  
  #####Layout susy的布局由各种混合的设置构成，Layouts存储为键值对的map格式，但也可以写成简短模式
  
  #####Layout 函数
  
		格式：layout($layout)
        $layout:	<layout>
 #       
  		// function input 输入
        $map: layout(auto 12 .25 inside fluid isolate);

        //output 输出
        $map: (
          container: auto,
          columns: 12,
          gutters: .25,
          gutter-position: inside,
          math: fluid,
          output: isolate,
        );

当你为susy设置不同变量组合的时候这是非常有用的，但是不能将两个简短变量组合在一起

		//不能正常工作
		$short: 12 .25 inside; //简写变量
		@include layout($short fluid no-gutters);

但可以添加一个map

        // 正常工作
        $map: layout(12 .25 inside);
        @include layout($map fluid no-gutters);

或者添加两个map

        $map1: 13 static;
        $map2: (6em 1em) inside;
        @include layout($map1 $map2);
        
###Layout 混入

     Format:	layout($layout, $clean)
     $layout:	<layout>
     $clean:	<boolean>
  

- - -

  
          // mixin: set a global layout 设置一个全局布局
        @include layout(12 1/4 inside-static);
        
###默认情况下这些设置都会添加到已有的全局变量当中去，使用$clean参数，另起炉灶，使当前设置为全局变量，弃用原有的全局变量设置

###with Layout 为模块临时设置默认值
    Format:	with-layout($layout, $clean) { @content }
    $layout:	<layout>
    $clean:	<boolean>
    @content:	Sass content block
        
_ _ _

* * *

- - -        
    @include with-layout(8 static) {
      // Temporary 8-column static grid...
    }

    // Global settings are restored...
        
 ###Susy-Get 获取全局变量的值

    Format:	susy-get($key, $layout)
    $key:	Setting name
    $layout:	<layout>
- - -

    $large: layout(80em 24 1/4 inside);
    $large-container: susy-get(container, $large);
- - -
获取一个嵌套的设置比如debug/image,需将路径以list的方式传入

    $debug-image: susy-get(debug image);
    
如果susy-get不传任何参数，则返回用户设置的susy全局变量


- - -

#变量属性

###flow 布局的浮动
	    Key:	flow
    Scope:	global, local
    Options:	rtl | ltr
    Default:	ltr

    ltr
        Layout elements will flow from left to right. //左浮动
    rtl
        Layout elements will flow from right to left. //右浮动
        
 ###Math susy可用于相对宽度和固定宽度
 	Key:	math
    Scope:	global, local
    Options:	fluid | static
    Default:	fluid
    
>     
fluid 流式布局
	对容器内的所有栅格的计算采用相对宽度的方式，输出百分比的值
    All internal grid spans will be calculated relative to the container, and output as % values./
static 固定布局
	所有栅格的值根据column-width计算，如果column的值设置为4em,输出值的单位也为em
    All internal grid values will be calculated as multiples of the column-width setting. If you set column-width to 4em, your grid widths will be output as em values. 

###output 布局输出 浮动布局和isolation布局（孤立布局）

	 Key:	output
    Scope:	global, local
    Options:	float | isolate
    Default:	float

> 
   float
   浮动布局
        Floats are the most common form of layout used on the web.
    isolate
    （解决像素约取错误）
        Isolation is a trick developed by John Albin Wilkins to help fix sub-pixel rounding bugs in fluid, floated layouts. You can think of it like absolute positioning of floats. We find it to be very useful for spot-checking the worst rounding bugs, but we think it’s overkill as a layout technique all to itself. 
	
###Container 设置容器的宽度

    Key:	container
    Scope:	global, local [container only]
    Options:	<length> | auto
    Default:	auto
    
 ==   对于固定宽度布局，设置container 为auto,设置column-width为具体值==
 
 
 ###container Position 容器位置
 
	setting
    Key:	container-position
    Scope:	global, local [container only]
    Options:	left | center | right | <length> [*2]
    Default:	center


>    
 left  : margin-left: 0; and margin-right: auto;.
center : margins to auto.
right  : margin-right: 0; and margin-left: auto;.
length [*2] exp： 10% 20% 表示 margin-left:10% margin-right:20%


###Columns 设置列数
	Key:	columns
	Scope:	global, local
	Options:	<number> | <list>
	Default:	4
	


>number 布局的列数
    The number of columns in your layout.
list 非对称上个布局  1 单列 （1,2） 2列，第二列的宽度为第二列的两倍
    For asymmetrical grids, list the size of each column relative to the other columns, where 1 is a single column-unit. (1 2) would create a 2-column grid, with the second column being twice the width of the first. For a Fibonacci-inspired grid, use (1 1 2 3 5 8 13). 
	

###Gutters 沟槽 设置列之间的间隔宽度

	Key:	gutters
	Scope:	global, local
	Options:	<ratio>
	Default:	1/4

>ratio 沟槽的比例 默认为1/4，沟槽宽度为colmun宽度的1/4，在非对称栅格布局当中为单列宽度的1/4
Gutters are established as a ratio to the size of a column. The default 1/4 setting will create gutters one quarter the size of a column. In asymmetrical grids, this is 1/4 the size of a single column-unit.
If you want to set explicit column and gutter widths, write your gutters setting as <gutter-width>/<column-width>. You can even leave the units attached.
// 70px columns, 20px gutters
$susy: (
  gutters: 20px/70px,
);


###Column Width 设置单列的宽度
		Key:	column-width
		Scope:	global, local
		Options:	<length> | false/null
		Default:	false

>length 用于固定宽度布局
    The width of one column, using any valid unit. This will be used in static layouts to calculate all grid widths, but can also be used by fluid layouts to calculate an outer maximum width for the container.
false/null 当使用 流式布局时设置为null,flase
    There is no need for column-width in fluid layouts unless you specifically want the container-width calculated for you. 
	
###Gutter Position 设置沟槽的位置
    Key:	gutter-position
    Scope:	global, local
    Options:	before | after | split | inside | inside-static
    Default:	after

>before  沟槽位于元素左边，每一行的第一个元素的沟槽需要被移除
after	 沟槽位于元素右边，每一行的最后个元素的沟槽需要被移除
split 沟槽运用margin位于每一个元素的两边，两边的元素沟槽没有被移除
inside 沟槽运用padding，位于元素两边，两边元素沟槽没有被移除
inside-static 固定宽度沟槽，运用于元素两边，两边元素宽度没有被移除
    

###Global Box Sizing 全局盒模型
    Key:	global-box-sizing
    Scope:	global
    Options:	border-box | content-box
    Default:	content-box

content-box 浏览器默认内容盒模型
border-box 边框盒模型


###Last Flow  一行最后一个元素的浮动

    Key:	last-flow
    Scope:	global
    Options:	from | to
    Default:	to

from 在ltr的浮动布局中，最后一个元素也为float:left
to  在ltr的浮动布局中，最后一个元素为float:right




#Debug 调试
    Key:	debug
    Scope:	global, local [container only]
    Options:	<map of sub-settings>
- - -
	$susy: (
	  debug: (
		image: show,  //调试图片是否显示
		color: blue,  // 调试图片颜色
		output: overlay, // 调试模式，背景 和 覆盖
		toggle: top right, // 调试按钮显示位置
	  ),
	);


 #Custom Support 自定义支持

		Key:	use-custom clearfix
		Scope:	global
		Options:	<boolean>
		Default:	false

		false susy 使用自带的clearfix
		true susy使用自定义的clearfix，如果不存在，则使用自带clearfix

#####Custom Background Image 自定义背景图片，用于debugging
#####Custom Background Options 自定义背景选项  用于debugging

#####ustom Breakpoint Options 
    Key:	use-custom breakpoint
    Scope:	global
    Options:	<boolean>
    Default:	true

	false 使用自带
	true 使用存在的bearkpoint,如果没有找到，则使用自带
	

###Custom Box Sizing  是否使用全部box-sizing mixin
    Key:	use-custom box-sizing
    Scope:	global
    Options:	<boolean>
    Default:	true

>false
    Susy will output box-sizing official syntax, as well as -moz and -webkit prefixed versions.
true
    Susy will look for an existing box-sizing mixin (like the ones provided by Compass and Bourbon), and fallback to mozilla, webkit, and official syntax if none is found. 


###Custom Rem ...



###Location ...


#####Box Sizing 为给定的元素设置盒模型
    Key:	box-sizing
    Scope:	local
    Options:	border-box | content-box
    Default:	null
	
	border-box
	content-box

  
###Spread 调整沟槽

    Scope:	local
    Options:	narrow | wide | wider
    Default:	various...

>narrow
    In most cases, column-spans include the gutters between columns. A span of 3 narrow covers the width of 3 columns, as well as 2 internal gutters. This is the default in most cases.
wide
    Sometimes you need to include one side gutter in a span width. A span of 3 wide covers the width of 3 columns, and 3 gutters (2 internal, and 1 side). This is the default for several margin/padding mixins.
wider
    Sometimes you need to include both side gutters in a span width. A span of 3 wider covers the width of 3 columns, and 4 gutters (2 internal, and 2 sides). 
  
  
###Gutter Override 设置一个元素的沟槽

    Key:	gutter-override
    Scope:	local
    Options:	no-gutters/no-gutter | <length>
    Default:	null

	no-gutters
	no-gutter
	  移除沟槽

  	length 覆写沟槽宽度

####Role
    Key:	role
    Scope:	local
    Options:	nest
    Default:	null
nest
    Mark an internal grid element as a context for nested grids.

Note
This can be used with any grid type, but it is required for nesting with split, inside, or inside-static gutters.

  
  
