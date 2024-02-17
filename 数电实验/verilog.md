``` verilog
module top_module( output one );
	
	assign one = 1'b1;
	
endmodule


module top_module ( output zero );
	
	assign zero = 1'b0;
	
endmodule

```

### 整数数值表示方法

数字声明时，合法的基数格式有 4 中，包括：十进制 ('d 或 'D)，十六进制 ('h 或 'H)，二进制（'b 或 'B），八进制（'o 或 'O）。数值可指明位宽，也可不指明位宽。

**指明位宽：**

``` verilog
## 实例
4'b1011         // 4bit 数值  
32'h3022_c0de   // 32bit 的数值  

其中，下划线 _ 是为了增强代码的可读性。

**不指明位宽:**

一般直接写数字时，默认为十进制表示，例如下面的 3 种写法是等效的：
```

``` verilog
## 实例

counter = 'd100 ; //一般会根据编译器自动分频位宽，常见的为32bit  
counter = 100 ;  
counter = 32'h64 ;
```

``` Verilog
### 实数表示方法

实数表示方法主要有两种方式：

**十进制：**

30.123
6.0
3.0
0.001

**科学计数法：**

1.2e4         //大小为12000
1_0001e4      //大小为100010000
1E-3          //大小为0.001
```

```verilog
module top_module(
	input [2:0] vec, 
	output [2:0] outv,
	output o2,
	output o1,
	output o0
);
	
	assign outv = vec;

	// This is ok too: assign {o2, o1, o0} = vec;
	assign o0 = vec[0];
	assign o1 = vec[1];
	assign o2 = vec[2];
	
endmodule



module top_module ( 
    input p1a, p1b, p1c, p1d, p1e, p1f,
    output p1y,
    input p2a, p2b, p2c, p2d,
    output p2y );
	wire in11,in21,in22;
	assign in11 = p1a&p1b&p1c;
	assign in12 = p1f&p1e&p1d;
	assign in21 = p2a&p2b;
	assign in22 = p2c&p2d;
	assign p1y = in11|in12;
	assign p2y = in21|in22;
endmodule



```

使用花括号级联
```verilog
Concatenation needs to know the width of every component (or how would you know the length of the result?). Thus, {1, 2, 3} is illegal and results in the error message: unsized constants are not allowed in concatenations.

The concatenation operator can be used on both the left and right sides of assignments.

input [15:0] in;
output [23:0] out;
assign {out[7:0], out[15:8]} = in;         // Swap two bytes. Right side and left side are both 16-bit vectors.
assign out[15:0] = {in[7:0], in[15:8]};    // This is the same thing.
assign out = {in[7:0], in[15:8]};       // This is different. The 16-bit vector on the right is extended to
                                        // match the 24-bit vector on the left, so out[23:16] are zero.
                                        // In the first two examples, out[23:16] are not assigned.
```




```verilog
	assign {o2, o1, o0} = vec;
```
![[Pasted image 20220929213322.png]]
## 片选 part select
```verilog

module top_module( 
    input [2:0] a,
    input [2:0] b,
    output [2:0] out_or_bitwise,
    output out_or_logical,
    output [5:0] out_not
);
    assign out_or_bitwise = a | b;
    assign out_or_logical = a || b;

    assign out_not[2:0] = ~a;	// Part-select on left side is o.
    assign out_not[5:3] = ~b;	//Assigning to [5:3] does not conflict with [2:0]
endmodule
```
>logical operator => || &&
>bitwise => | &

## 让整个变量bitwise

```verilog
module top_module( 
		input [3:0] in, 
		output out_and, 
		output out_or, 
		output out_xor ); 
		assign out_and = & in; 
		assign out_or  = | in; 
		assign out_xor = ^ in; 
endmodule
```

