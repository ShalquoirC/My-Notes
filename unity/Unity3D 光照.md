# Unity3D 光照

Directlight

transform

### light

baking(烘培)

color（颜色）

Intensity(强度)

Bounce Intensity(反射强度)

Shadows（阴影)

Cookies(遮挡)

Render Mode(渲染模式)

Cilling Mask（剔除遮挡)

* 可以选择性（层）照亮
* 可以用于照亮角色（显示轮廓）
* 天花板、墙面均匀光照



#### Area Light

仅作用于baking

##### Mesh Renderer

###### **属性**

| **属性：**              |                          **功能：**                          |
| :---------------------- | :----------------------------------------------------------: |
| **Light Probes**        |                   基于探针的光照插值模式。                   |
| Off                     |                渲染器不使用任何插值光照探针。                |
| Blend Probes            |          渲染器使用一个插值光照探针。这是默认选项。          |
| Use Proxy Volume        |              渲染器使用插值光照探针的 3D 网格。              |
| **Reflection Probes**   | 指定游戏对象如何受场景中的反射影响。不能在延迟渲染模式下禁用此属性。 |
| Off                     |               禁用反射探针，将天空盒用于反射。               |
| Blend Probes            | 启用反射探针。混合仅发生在探针之间，在室内环境中非常有用。如果附近没有反射探针，则渲染器将使用默认反射，但默认反射和探针之间不会发生混合。 |
| Blend Probes and Skybox | 启用反射探针。混合发生在探针之间或探针与默认反射之间，对于室外环境非常有用。 |
| Simple                  | 启用反射探针，但当存在两个重叠的探针体积时，探针之间不会发生混合。 |
| **Anchor Override**     |    使用**光照探针**或反射探针系统时用变换来确定插值位置。    |
| **Cast Shadows**        |                                                              |
| On                      |              阴影投射光源照在网格上时将投射阴影              |
| Off                     |                       网格不会投射阴影                       |
| Two Sided               | 从网格的任一侧投射双面阴影。Enlighten 和渐进光照贴图 (Progressive Lightmapper) 不支持双面阴影。 |
| Shadows Only            |              网格的阴影将可见，但网格本身不可见              |
| **Receive Shadows**     | 启用此复选框可使网格显示任何投射在网格上的阴影。仅当使用渐进光照贴图时才支持 Review Shadows 选项 |
| **Motion Vectors**      | 如果启用此属性，则线会将运动矢量渲染到摄像机运动矢量纹理中。 |
| **Lightmap Static**     | 启用此复选框可向 Unity 指示该游戏对象的位置是固定的并将参与全局光照计算。如果某个游戏对象未标记为 Lightmap Static，则仍可使用**光照探针**为该对象提供光照。 |
| **Materials**           |                   用于渲染模型的材质列表。                   |
| **Dynamic Occluded**    | 启用此复选框可向 Unity 指示即使该对象未标记为静态，仍应该对该游戏对象执行遮挡剔除。 |

*小物品可以关闭阴影，减少计算量*



#### 伪造阴影（绘制

贴图（ps绘制阴影）

选择目标位置 羽化 填充 正片叠底不透明度

#### 投影器

*effect*  *projector*

绘制阴影 

ps  

不均匀的阴影

* Color Dynamics（动态颜色）
* Scattering（散布）

导出png后

1.导入unity 将Texture Type 改为Advanced（高级）

2.Wrap Mode（循环模式）改为Clamp（强制拉伸）

3.勾选Border Mip Maps（边界多级渐进纹理）*消除奇怪的边界线*

4.在光照纹理中 点击select 找到对应的伪阴影。

5.应用，将点**投影器**移动到目标的位置 

6.将物体和**投影器**绑定