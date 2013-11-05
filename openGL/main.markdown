#openGL

##硬件流水线
>顶点数据生成 vertex puller → 顶点数据处理 vertex shader → 图元生成 tesselletion control shader + tesselletion primitive gen.
+ tessellation Eval. shader → 图元处理 geometry shader → 片元生成（光栅化） rasterization → 片元处理 fragment shader → 像素更新 per-fragment 
operations
