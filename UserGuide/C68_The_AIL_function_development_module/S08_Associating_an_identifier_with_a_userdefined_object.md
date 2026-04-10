---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: Associating_an_identifier_with_a_userdefined_object
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / Associating an identifier with a userdefined object"
---

# Associating an Aurora Imaging Library identifier with a user-defined object

The library's Function Development module allows you to associate an Aurora Imaging Library identifier with an object so that this object can be treated as a standard Aurora Imaging Library object. Use the [`MfuncAllocId`](../../Reference/func/MfuncAllocId.md) function to associate an Aurora Imaging Library identifier with any array or custom data structure. Defining your own user-defined Aurora Imaging Library objects allows you to create data objects whose exact internal structure has specific requirements and whose data members should not be accessed directly. Once you have associated the user-defined object with an Aurora Imaging Library identifier, it is known as a user-defined Aurora Imaging Library object, and it will be subject to error checking and tracing as any other Aurora Imaging Library object.

Typically, you would create user-defined Aurora Imaging Library objects in a user-defined Aurora Imaging Library function. In this case, it is recommended that you create a user-defined Aurora Imaging Library function whose sole purpose is to allocate the user-defined object and associate it with an Aurora Imaging Library identifier. That is, the callee function of the user-defined Aurora Imaging Library function only declares the object and associates it with an Aurora Imaging Library identifier. This type of function is called a user-defined Aurora Imaging Library allocation function. User-defined Aurora Imaging Library allocation functions should be allocated using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) with [`M_ALLOC`](../../Reference/func/MfuncAlloc.md).

It is recommended that you register an Aurora Imaging Library system identifier as a parameter of your user-defined Aurora Imaging Library allocation function ([`MfuncParamAilId`](../../Reference/MfuncParamAilId.md) with [`AilObjectType`](../../Reference/func/MfuncParam.md) set to [`M_SYSTEM`](../../Reference/func/MfuncParam.md)); this allows you to easily control which system your function will be executed on at runtime, and, by extension, on which system your user-defined object will be allocated. The Aurora Imaging Library identifier associated with your newly allocated user-defined object can be retrieved by having your allocation function return it, or by storing it in a pointer parameter of type AIL_ID ([`MfuncParamArrayAilId`](../../Reference/MfuncParamArrayAilId.md)).

> **Note:** Note that user-defined objects stored in the memory of a remote processor are not accessible from the Host processor, and vice versa, unless they are stored in shared memory.

To refer to user-defined Aurora Imaging Library objects, use their Aurora Imaging Library identifiers. To refer to the actual data grouped in this user-defined Aurora Imaging Library object, use a pointer to the object. You can retrieve the address of the object using the [`MfuncInquire`](../../Reference/func/MfuncInquire.md) function with [`M_OBJECT_PTR`](../../Reference/func/MfuncInquire.md).

For example, for an object with this structure:

> **Code example:** [userguide.The_AIL_function_development_module.associating_a_ail_identifier_with_a_user-define_object01](userguide.The_AIL_function_development_module.associating_a_ail_identifier_with_a_user-define_object01)

The following portion of Aurora Imaging Library code shows how to allocate a custom object and associate it with an Aurora Imaging Library identifier; this type of code would typically be found in a user-defined allocation function.

> **Code example:** [userguide.The_AIL_function_development_module.associating_a_ail_identifier_with_a_user-define_object02](userguide.The_AIL_function_development_module.associating_a_ail_identifier_with_a_user-define_object02)

The following portion of Aurora Imaging Library code shows how to retrieve the address of the user-defined Aurora Imaging Library object, allocated above, and use it to store data using a pointer; this type of code would typically be found in a user-defined Aurora Imaging Library processing function which accepts the Aurora Imaging Library object as a parameter.

> **Code example:** [userguide.The_AIL_function_development_module.associating_a_ail_identifier_with_a_user-define_object03](userguide.The_AIL_function_development_module.associating_a_ail_identifier_with_a_user-define_object03)

When creating a user-defined Aurora Imaging Library object, you must specify the Aurora Imaging Library type of your object. Aurora Imaging Library provides two object type groups ([`M_USER_OBJECT_1`](../../Reference/func/MfuncAllocId.md) and [`M_USER_OBJECT_2`](../../Reference/func/MfuncAllocId.md)), permitting you to distinguish between categories of custom created objects. Each user-defined object group can contain up to 16 user-defined object types ([`M_USER_OBJECT_n`](../../Reference/func/MfuncAllocId.md) `+ _m_`, where _n_ is 1 or 2, and _m_ specifies the offset within the group).

To inquire about the type of a user-defined Aurora Imaging Library object, use the [`MobjInquire`](../../Reference/obj/MobjInquire.md) function with [`M_OBJECT_TYPE`](../../Reference/obj/MobjInquire.md).

Once you have finished using a user-defined Aurora Imaging Library object, you must free the Aurora Imaging Library identifier associated with this object, using the [`MfuncFreeId`](../../Reference/func/MfuncFreeId.md) function. Note that you need not free the Aurora Imaging Library identifier in the same function where it was associated with the object ([`MfuncAllocId`](../../Reference/func/MfuncAllocId.md)). It is recommended that you create a user-defined Aurora Imaging Library function whose sole purpose is to free the user-defined object and its Aurora Imaging Library identifier. This type of function should be allocated using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) with [`M_FREE`](../../Reference/func/MfuncAlloc.md), and must accept the Aurora Imaging Library identifier to be freed as its only parameter.
