---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Containers
section: Hooking_to_containers
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Containers / Hooking to containers"
---

# Hooking to containers

You can hook a callback function to changes in a container using [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md). You can choose to hook to either the container itself, or to all components of the container individually (including components allocated in the future). For more information about hook-handler functions in general, see [Event handling in Aurora Imaging Library with hook-handler functions](../C02_Building_an_application/S18_Event_handling.md).

If you hook to the container itself, the callback function will be called after any Aurora Imaging Library function that modifies at least one of its components, adds or removes a component, or uses the container as a destination (even if the container is not modified). Using [`MbufGetHookInfo`](../../Reference/buf/MbufGetHookInfo.md)with [`M_MODIFIED_BUFFER`](../../Reference/buf/MbufGetHookInfo.md)within the callback function will return the identifier of the container. The callback function will never be called more than once for a single Aurora Imaging Library function call.

If you hook to all components of the container (by specifying[`M_ON_COMPONENT`](../../Reference/buf/MbufHookFunction.md)), the callback function will be called after any Aurora Imaging Library function that modifies one of those components (including allocating or freeing the component). Using [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md)with [`M_MODIFIED_BUFFER`](../../Reference/buf/MbufHookFunction.md)within the callback function will return the identifier of the component. If a single function modifies multiple components of the container (for example, if the container is a destination for a 3D processing function that modifies or frees multiple components), the callback function will be called once for each modified component.

If you manually specify that a container has been modified (using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md) with [`M_MODIFIED`](../../Reference/buf/MbufControlContainer.md)), Aurora Imaging Library will also call functions hooked to the container's components. If you do this for a child container, Aurora Imaging Library will also call functions hooked to the parent container(s).
