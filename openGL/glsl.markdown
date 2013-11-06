##shader代码的装载
+ shader代码可以用文本文件保存，可以在程序中用字符串保存，但最终还是必须在程序中以字符串的形式传入Shader对象中；
+ 创建Shader对象（glCreateShader——成功时返回非0的无符号Handle值，指涉Shader对象）；
+ 把shader代码传入shader对象（glShaderSource——注意此函数的参数，字符流地址参量是GLchar*，不支持宽字符。
执行后可删除内存上保存的shader代码字符串副本）；
+ 编译Shader对象——正如我们编译任何代码一样（glCompileShader——应该以GL_COMPILE_STATUS为参调用glGetShaderiv检查编译是否成功。
如果代码出现问题会在这个阶段报错，debug时可用glGetError查看更具体的错误类型）；
+ 按以上步骤把一个“过程”中需要的各类型shader编译好（通常每种类型最多建一个shader对象——因为对于一个渲染管道（流程）来说，
顶点、单几何体、像素都只处理一遍，不可能返回。
shaders只是插入这个流程中取代对应的固定管道处理的“外挂”[在抹除固定管道处理、全shader时代来临之前可以这么说]，
在一个渲染流程中起作用的shaders姑且统称为一个shader过程）；

>OpenGL通过一个名为shaderProgram的对象来与shader交互。也可以说shaders通过这个对象连接到我们的OpenGL应用程序中，
它具体地指涉shader过程。宏观概念上类似于我们平时写的“程序”，不过它是基于GPU的；

+ 创建这么一个shaderProgram对象（glCreateProgram——成功时返回非0的无符号Handle值，指涉Shader程序对象）；
+ 把之前创建的shaders，一个一个地Attach到这个shaderProgram对象上（glAttachShader——当然理论上可以attach多于一个的同类型完整的shader
，譬如vertexShader。但是在一个特定的渲染流程中只允许其中一个起作用，你明白的）；
+ 链接shaderProgram——正如我们链接任何程序一样（glLinkProgram——应该以GL_LINK_STATUS为参调用glGetProgramiv检查链接是否成功。）

至此一个shader程式装载完毕。在进入一批渲染流程前（即渲染一组物件前）启用这个shaderProgram对象（glUseProgram），它就会在这批渲染流程中起作用了。渲染完后可以（也应该）调用glUseProgram(NULL)来关闭这种介入，或者以其他shaderProgram渲染别的物件。



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
