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


		嵌套函数可以帮助我们更简单的处理上下文环境，而不需要自己计算

    $grid: (1 2 3 2 1);

    .outer {
      @include span(3 of $grid at 2);

      $context: nested(3 of $grid at 2);
      .inner { @include span(2 of $context at 1); }
    }











