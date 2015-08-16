#Toolkit 
###span [mixin] 设置布局元素

    mixin
        Format:	span($span) { @content }
        $span:	<span>
        @content:	Sass content block

    Arbitrary Widths 传递一个任意的宽度
    // arbitrary width
    .item { @include span(25%); }  //输入

    // float output (without gutters) // 输出
    .item {
      float: left;
      width: 25%;
    }
    
    
###Grid Widths  如果使用栅格

    / grid span
    .item { @include span(3); } 输入

    // output (7-column grid with 1/2 gutters after) 输出
    .item {
      float: left;
      width: 40%;
      margin-right: 5%;
    }
    
###Row Edges 当使用参数before和after参数来决定沟槽位置时，你有时候需要设置first或last来移除两边元素的沟槽

    // grid span
    @include span(last 3);

    // output (same 7-column grid)
    .item {
      float: right;
      width: 40%;
     margin-right: 0; 
    }


###context 上下文环境 。 当使用流式布局或者在其他元素中嵌套栅格时都需要Context

    .outer {
      @include span(5);
      .inner { @include span(2 of 5); }
    }
- - -
     of 标识用于单个上下文环境，上下文环境一般等于父元素的栅格跨度，根据元素的嵌套你可以推断出上下文的变化
     // 10-column grid
    .outer {
      // out here, the context is 10
      @include span(5) {  上下文环境为10个栅格
        // in here, the context is 5  上下文栅格为5个栅格
        .inner { @include span(2); }
      }
    }
    
###Nesting 栅格使用split,inside inside-static 沟槽时，不需要担心边界情况，但是你需要考虑到嵌套
如果一个元素当中有栅格对齐的子元素时，需要标识为nest

    // inside, inside-static, or split gutters
    .outer {
      @include span(5 nest);
      .inner { @include span(2 of 5); }
    }

###Location
	使用非对称网格布局或者绝对定位布局时，需要知道栅格位置的设置，此时可以使用at 标识定位
    对于绝对定位，可以使用任意的宽度或者列的序列号，使用非对称栅格布局 时，必须是栅格的序列号

    .width { @include span(isolate 500px at 25%); } // 可以使用百分比
    .index { @include span(isolate 3 at 2); } // 使用栅格的序列号
    
    
###narrow, wide, and wider 
    By default, a grid span only spans the gutters between columns. So a span of 2 includes 1 internal gutter (narrow). In some cases you want to span additional gutters on either side. So that same span of 2 could include the internal gutter, and one (wide) or both (wider) external gutters.

    // grid span
    .narrow { @include span(2); }
    .wide { @include span(2 wide); }
    .wider { @include span(2 wider); }

    // width output (7 columns, .25 gutters)
    // (each column is 10%, and each gutter adds 2.5%)
    .narrow { width: 22.5%; }
    .wide { width: 25%; }
	.wider { width: 27.5%; }
    
    
#Other Settings 其他设置
	// grid span
    .item { @include span(isolate 4 at 2 of 8 (4em 1em) inside rtl break); }

    // output
    .item {
      clear: both;
      float: right;
      width: 50%;
      padding-left: .5em;
      padding-right: .5em;
      margin-left: 25%;
      margin-right: -100%;
    }


###span 函数

        Format:	span($span)
        $span:	<span>

    .item {
      width: span(2);
      margin-left: span(3 wide);
      margin-right: span(1) + 25%;
    }
###Gutters


function/mixin
    Format:	gutters($span)
    Alternate:	gutter($span)
    $span:	<span>

	使用gutter或gutters 返回当前环境下设置的沟槽的宽度

    // default context  默认的上下文环境
    margin-left: gutter();

    // nested in a 10-column context  嵌套在10列布局的上下文环境
    margin-left: gutter(10);
- - -
	使用混入方式，沟槽根据gutter-posiiton的设置以margin或padding方式输出

    // default gutters 默认的沟槽
    .item { @include gutters; }

	也可以传递一个具体的值
    // explicit gutters
    .item { @include gutters(3em); }

	或以简写的形式动态调整设置
    // inside gutters 内部沟槽
    .item { @include gutters(3em inside); }

    // gutters after, in an explicit (10 1/3) layout context 在10列布局的上下文环境，沟槽宽度为1/3
    .item { @include gutters(10 1/3 after); }
    
    
###Container 容器

    function/mixin
    Format:	container($layout)
    $layout:	<layout>

	根据一个可选的布局参数或全局设置，返回一个容器的宽度值
    // global settings 返回全局设置的容器宽度值
    width: container();

    // 12-column grid 12列栅格宽度
    $large-breakpoint: container(12);

   使用混入直接设置容器宽度

    body {
      @include container(12 center static);
    }

    Note that static math requires a valid column-width setting
    
    
    
### Nested Context 嵌套上下文

    function/mixin
        Function:	nested($span)
        Mixin:	nested($span) { @content }
        $span:	<span>
        @content:	Sass content block

    Sass 不会注意DOM或者你站点的构成。所以susymixins并不知道子元素或祖先元素之间的关系，所以当你在容器创建一个山歌不同于默认的栅格设置，你需要为嵌套元素传递一个准确的上下文
    
	以简写的方式传递上下文
    body { @include container(8); }
    .span { @include span(3 of 8); }

	nested mixin 以简写的方式为一个模块提供上下文
    @include nested(8) {
      .span { @include span(3); }
    }

		当非对称山歌布局时，需要知道列的数目和哪一列可用
        
    .outer {
      @include span(3 of (1 2 3 2 1) at 2);
      //从第二列开始的后三列 即 2 3 2 =7  7/9  宽度为7/9 

      // context is now (2 3 2)...
      .inner { @include span(2 of (2 3 2) at 1); }
    }
	// 从第一列开始的两列 即2 3=5 5/7的宽度

- - -
		嵌套函数可以帮助我们更简单的处理上下文环境，而不需要自己计算

    $grid: (1 2 3 2 1);

    .outer {
      @include span(3 of $grid at 2);

      $context: nested(3 of $grid at 2);
      .inner { @include span(2 of $context at 1); }
    }

###Global Box Sizing
为全局样式设置盒模型

    mixin
        Format:	global-box-sizing($box [, $inherit])
        Shortcut:	border-box-sizing([$inherit])
        $box:	content-box | border-box
        $inherit:	[optional] true | false

设置可选参数$inherit 为true,默认继承全局盒模型样式，但是可以用元素自己的box-sizng轻易覆写。一旦为对象设置了盒模型，所有嵌套的元素都会被继承，除非inherit设置为false

你可以传递一个box-sizing 参数 为一个元素设置盒模型

    // input
    .item { @include span(25em border-box); }

    // sample output (depending on settings)
    .item {
      float: left;
      width: 25em;
      box-sizing: border-box;
    }


 我们强烈推荐使用border-box盒模型，尤其是在当你使用内部沟槽时

    // the basics with default behavior:
    * { box-sizing: border-box; }

    // the basics with $inherit set to true:
    html { box-sizing: border-box; }
    * { box-sizing: inherit; }
  
susy 需要设置盒模型，所以最好的方式是以简写方式设置全局盒模型
    // the flexible version:
    @include global-box-sizing(border-box);

    // the shortcut:
    @include border-box-sizing;


#Rows & Edges 行和边界

    mixin
        Format:	break()
        Reset:	nobreak()
        Keywords:	break | nobreak

		创建新行，需要清除之前的浮动

    .new-line { @include break; }

	当你要重置清除浮动，设置clear:none;时，使用关键字nobreak关键字
    .no-new-line { @include nobreak; }

    Both break and nobreak can also be used as keywords with the span mixin.

###First

    mixin
        Format:	first($context)
        Alternate:	alpha($context)
        $context:	<layout>

    Note 仅当在gutter-position 设置为before时有效 ，移除每行第一个元素的沟槽


    .first { @include first; }

###last 同上


###full  占据整行
	.last { @include full; }
    
#margins

###pre 根据浮动方向在元素前面添加一个margin
###post 根据浮动方向在元素之后添加一个margin
###pull 根据浮动方向在元素之前添加一个负margin
###squish  同时为元素添加前后margin

盒模型影响元素width和padding的关系，浏览器默认盒模型box-content,宽度由padding和width共同决定，推荐的border-box盒模型，padding为width的一部分，宽度由width决定
#padding

###prefix 根据元素浮动情况，在元素之前添加一个padding
###suffix 根据元素浮动情况，在元素之后添加一个padding
###pad 同时为元素添加前后padding
###bleed 出血 ，添加一个负margin和正的padding，元素内容超出了容器范围 ，但不影响内容布局
	.example1 { @include bleed(1em); }
    .example2 { @include bleed(1em 2 20px 5% of 8 .25); }

    // output
    .example1 {
      margin: -1em;
      padding: 1em;
    }

    .example2 {
      margin-top: -1em;
      padding-top: 1em;
      margin-right: -22.5%;
      padding-right: 22.5%;
      margin-bottom: -20px;
      padding-bottom: 20px;
      margin-left: -5%;
      padding-left: 5%;
    }

###bleed-x  前后出血
###bleed-y 上下出血
###isolate 基于浮动的布局技术，参数和span相同，但是任何长度值和栅格序列都会被解释为孤立定位
    // input
    .function {
      margin-left: isolate(2 of 7 .5 after);
    }

    // output
    .function {
      margin-left: 15%;
    }

    And the mixin returns all the properties required for isolation.

    // input
    .mixin { @include isolate(25%); }

    // output
    .mixin {
      float: left;
      margin-left: 25%;
      margin-right: -100%;
    }


###Gallery 画廊样式，使用子元素选择器和孤立布局技术对元素进行布局

    // each img will span 3 of 12 columns,
    指定图片元素宽度
    // with 4 images in each row:
    每一行有四张图片 
    inner
    .gallery img {
      @include gallery(3 of 12);
    }
        output

    .outer {
      background: green;
      width: 23.72881%;
      float: left;
    }

    .outer:nth-child(4n + 1) {
      margin-left: 0;
      margin-right: -100%;
      clear: both;
      margin-left: 0;
    }

    .outer:nth-child(4n + 2) {
      margin-left: 25.42373%;
      margin-right: -100%;
      clear: none;
    }

    .outer:nth-child(4n + 3) {
      margin-left: 50.84746%;
      margin-right: -100%;
      clear: none;
    }

    .outer:nth-child(4n + 4) {
      margin-left: 76.27119%;
      margin-right: -100%;
      clear: none;
    }

    
    
###show Grid ...

### susy BreakPoint  断点

	Format:	susy-breakpoint($query, $layout, $no-query)
    $query:	media query shorthand (see susy-media)
    $layout:	<layout>
    $no-query:	<boolean> | <string> (see susy-media)
    
    susy-breakpoint() acts as a shortcut for changing layout settings at different media-query breakpoints, using either susy-media or the third-party Breakpoint plugin.

    If you are using the third-party plugin, see Breakpoint: Basic Media Queries and Breakpoint: No Query Fallbacks for details.

    This mixin acts as a wrapper, adding media-queries and changing the layout settings for any susy functions or mixins that are nested inside.
		inner
    @include susy-breakpoint(30em, 8) {
      // nested code uses an 8-column grid,
      嵌套8列的栅格，最新的断点为30em
      // starting at a 30em min-width breakpoint...
      .example { @include span(3); }
    }
	
    output
    @media (min-width: 30em) {
      .example {
        width: 35.89744%;
        float: left;
        margin-left: 2.5641%;
      }
    }


### Susy Media








