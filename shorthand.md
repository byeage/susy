#shorthand
susy提供了简写语法可以给函数和混入传递任意设置

    // Establish an 80em container
    @include container(80em);

    // Span 3 of 12 columns
    @include span(3 of 12);

    // Setup the 960 Grid System
    @include layout(12 (60px 20px) split static);

    // Span 3 isolated columns in a complex, asymmetrical grid, without gutters
    @include span(isolate 3 at 2 of (1 2 3 4 3 2 1) no-gutters);
    
 ###Overview语法 

 	Shorthand:	$span $grid $keywords;
    span 可以为任意的长度，或者无单位的数字表示所跨的列
    grid 由columns,可选的gutters,Column-width组成
    
    // 12-column grid
    $grid: 12; // 12列

    // 12-column grid with 1/3 gutter ratio
    $grid: 12 1/3; 12列 1/3列宽度的沟槽

    // 12-column grid with 60px columns and 10px gutters
    $grid: 12 (60px 10px); //12列每列宽度60px,沟槽宽度10px;

    // asymmetrical grid with .25 gutter ratio
    $grid: (1 2 3 2 1) .25; //非对称山歌布局 单列宽度的四分之一
    
    
###keywords
    $global-keywords: (
      container            : auto,
      math                 : static fluid,
      output               : isolate float,
      container-position   : left center right,
      flow                 : ltr rtl,
      gutter-position      : before after split inside inside-static,
      debug: (
        image              : show hide show-columns show-baseline,
        output             : background overlay,
      ),
    );

    $local-keywords: (
      box-sizing           : border-box content-box,
      edge                 : first alpha last omega,
      spread               : narrow wide wider,
      gutter-override      : no-gutters no-gutter,
      clear                : break nobreak,
      role                 : nest,
    );

### layout 简写的变体用于定义自己的布局
	Pattern:	<grid> <keywords>
    
    没有是必选项 所有的设置都为可选的拥有全局默认样式设置
    // grid: (columns: 4, gutters: 1/4, column-width: 4em);
    // keywords: (math: fluid, gutter-position: inside-static, flow: rtl);
    $small: 4 (4em 1em) fluid inside-static rtl;
    
    
#span
    Pattern:	<span> at <location> of <layout>

    // Pattern:
    $span: $span at $location of $layout;

    // span: 3;
    // location: 4;
    // layout: (columns: 12, gutters: .25, math: fluid)
    $span: 3 at 4 of 12 .25 fluid;

    // Only $span is required in most cases
    $span: 30%;


