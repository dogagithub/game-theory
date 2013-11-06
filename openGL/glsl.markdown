##三种变量类型（uniform,attribute,varying）

###uniform（不变的）
uniform变量是外部application程序传递给（vertex和fragment）shader的变量。
因此它是application通过函数glUniform**（）函数赋值的。
在（vertex和fragment）shader程序内部，uniform变量就像是C语言里面的常量（const ），它不能被shader程序修改。（shader只能用，不能改）

如果uniform变量在vertex和fragment两者之间声明方式完全一样，则它可以在vertex和fragment共享使用。
（相当于一个被vertex和fragment shader共享的全局变量）

uniform变量一般用来表示：变换矩阵，材质，光照参数和颜色等信息。

###attribute（属性）
attribute变量是只能在vertex shader中使用的变量。（它不能在fragment shader中声明attribute变量，也不能被fragment shader中使用）

一般用attribute变量来表示一些顶点的数据，如：顶点坐标，法线，纹理坐标，顶点颜色等。

在application中，一般用函数glBindAttribLocation（）来绑定每个attribute变量的位置，然后用函数glVertexAttribPointer（）为每个attribute变量赋值。
      uniform mat4 u_matViewProjection;
      attribute vec4 a_position;
      attribute vec2 a_texCoord0;
      varying vec2 v_texCoord;
      void main(void)
      {
      gl_Position = u_matViewProjection * a_position;
      v_texCoord = a_texCoord0;
      }
