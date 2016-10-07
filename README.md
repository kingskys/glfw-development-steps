# glfw-development-steps

摘自网络 http://blog.csdn.net/itol925/article/details/50096827

glfw开发步骤

1）引入头文件

\#include \<GLFW/glfw3.h>

glfw3头文件中，定义了GLFW API的所有的常量，类型和函数，也包含了opengl的头文件，和平台所需的常量。
在windows下，我们需要在include gl/gl.h之前include windows.h，这就影响到了我们代码的纯洁性。而在glfw中不需要考虑这些，不用手动添加，如果非要添加的话，需要将在include glfw3.h之前include windows.h。

在glfw3.0之后，glu.h在默认情况下，将不再引入了，如果非要引入的话，可以

\#define GLFW_INCLUDE_GLU

\#include \<GLFW/glfw3.h\>
 
 
2）初始化销毁glfw

调用glfwInit()初始化glfw，成功的话返回GL_TRUE，否则返回GL_FALSE
一般情况下，在程序exit之前，需要销毁glfw，调用glfwTerminate()


3）设置glfw的错误回调函数

glfwSetErrorCallback(error_callback);

void error_callback(int error, constchar* description)


4）创建窗口和上下文 

GLFWwindow* window = glfwCreateWindow(640, 480, "My Title", NULL, NULL);
如果创建失败的话返回NULL，注意，返回失败时应该调用glfwTerminate()
创建成功的话，返回的句柄会始终传递于glfw各函数及回调函数之间，如果需要销毁，可调
glfwDestoryWindow(window);

一旦窗口句柄被销毁，窗口的事件也将失效。


5）设置当前OpenGL上下文

glfwMakeContextCurrent(window);


6）检测窗口关闭标志，循环系统

while(!glfwWindowShouldClose(window)){
}

每个窗口都有一个是否关闭的flag。

每个也 可以设置关闭窗口的回调函数：glfwSetWindowCloseCallback。函数将在flag设置为1后立马调用。


7）接收输入事件

每个窗口都有大量的回调函数可以接收各种事件。

按键回调：glfwSetKeyCallback(window, key_callback);

static void key_callback(GLFWwindow* window, int key, int scancode, int action, int mods)
{
    if (key == GLFW_KEY_ESCAPE && action == GLFW_PRESS)
        glfwSetWindowShouldClose(window, GL_TRUE);
}


8）渲染OpenGL

当创建了一个OpenGL上下文之后，需要从重新获取framebuffer size设置给glViewport。

int width, height;

glfwGetFramebufferSize(window, &width, &height);

glViewport(0, 0, width, height);


9）读取时间

double time = glfwGetTime();

获取一个从glfw初始化完到当前的一个时间秒数


10）swapping buffers

glfwSwapBuffers(window);

可以设置缓存刷新时间：glfwSwapInterval(1);


11）处理事件

处理事件有2个函数：

glfwPollEvents()和glfwWaitEvents(),前者会立即处理已经到位的事件，后者等待。


参考：http://www.glfw.org/docs/latest/quick.html

