###compass 安装
		compass create --using susy <project name>

####开始

		@import "susy";

###基本布局由两个mixins构成
		@include container; // establish a layout context
		@include span(<width>); // lay out your elements
        
#####如果你想为元素运用网格布局，使用 span mixin 来计算行的宽度
		nav{@include span(3 of 12);}
        
#####也可使用更直接的数学方式
		main {
              float: left;
              width: span(4);
              margin-left: span(2) + gutter();
              margin-right: gutter();
            }
            
#####也可以根据需要为susy设置全局变量
		$susy: (
          columns: 12,  // The number of columns in your grid
          gutters: 1/4, // The size of a gutter in relation to a single column
        );





