### VkMemoryPropertyFlagBits
- VK_MEMORY_PROPERTY_DEVICE_LOCAL_BIT 					仅GPU可访问，CPU不可访问
- VK_MEMORY_PROPERTY_HOST_VISIBLE_BIT					GPU可访问，CPU可访问（vkMapMemory）
- VK_MEMORY_PROPERTY_HOST_COHERENT_BIT					立即同步vkMapMemory返回指针数据到GPU，没有该标记需手动同步数据（适合单次修改数据同步）
- VK_MEMORY_PROPERTY_HOST_CACHED_BIT					CPU建立数据缓存，没有该标志一定有VK_MEMORY_PROPERTY_HOST_COHERENT_BIT标记
- VK_MEMORY_PROPERTY_LAZILY_ALLOCATED_BIT = 0x00000010,
- VK_MEMORY_PROPERTY_PROTECTED_BIT = 0x00000020,

### VkAttachmentLoadOp
- VK_ATTACHMENT_LOAD_OP_LOAD = 0,						在RenderPass开始时，加载前一帧的图像到Frame上，要求可读BIT
- VK_ATTACHMENT_LOAD_OP_CLEAR = 1,						在RenderPass开始时，Pass传入的Color值填充Frame，要求可写BIT
- VK_ATTACHMENT_LOAD_OP_DONT_CARE = 2,					在RenderPass开始时，不关心Frame当前值，要求可写BIT（性能最好）