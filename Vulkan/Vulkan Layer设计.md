### 核心结构
- Loader: 连接Application和ICD的桥梁，负责 1、Layer的加载 2、vk API调用链转发到Instance层和对应IDC层
- Layer: optional component，用于各种扩展功能，掺入 Dispatch Tables 机制下hook函数调用以扩展功能（如 Debug Layer）
- Installable Client Drivers: 物理硬件在Application层上的标识(VkPhysicalDevice)
- Instance: vulkan sdk的顶层管理对象标识
- Devices: vulkan sdk的IDC在Application层的逻辑对象标识，一个ICD可以对应多个Devices（VkDevice）
- Extensions: 包括 swapchain（base graphic） win32 surface（base OS）