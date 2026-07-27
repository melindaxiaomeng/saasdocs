# SaaS Admin API 完整文档

> **版本**: 1.0.0  
> **描述**: 支持 RBAC + 数据权限 + 协作的企业级权限管理系统

---

## 目录

1. [认证 Auth](#认证-auth)
2. [用户管理 User](#用户管理-user)
3. [部门管理 Department](#部门管理-department)
4. [职务管理 Position](#职务管理-position)
5. [菜单集合 MenuCollection](#菜单集合-menucollection)
6. [菜单 Menu](#菜单-menu)
7. [权限集合 PermissionCollection](#权限集合-permissioncollection)
8. [权限 Permission](#权限-permission)
9. [资源类型 ResourceType](#资源类型-resourcetype)
10. [资源类型规则 ResourceTypeRule](#资源类型规则-resourcetyperule)
11. [资源范围 ResourceScope](#资源范围-resourcescope)
12. [资源范围详情 ResourceScopeDetail](#资源范围详情-resourcescopedetail)
13. [资源协作范围 ResourceCollaborationScope](#资源协作范围-resourcecollaborationscope)
14. [用户菜单 UserMenu](#用户菜单-usermenu)
15. [用户菜单集合 UserMenuCollection](#用户菜单集合-usermenucollection)
16. [用户权限 UserPermission](#用户权限-userpermission)
17. [用户权限集合 UserPermissionCollection](#用户权限集合-userpermissioncollection)
18. [用户资源范围 UserResourceScope](#用户资源范围-userresourcescope)
19. [用户资源协作 UserResourceCollaboration](#用户资源协作-userresourcecollaboration)

---

## 认证

### Auth

#### POST `/api/v1/auth/login`

**Login**

用户登录端点。

返回:
    访问令牌和刷新令牌

**Request Body**:
```json
LoginRequest
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_TokenResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### POST `/api/v1/auth/logout`

**Logout**

用户登出端点。

返回:
    成功消息

**Responses**:

- `200`: Successful Response
  ```json
object
  ```

---

#### POST `/api/v1/auth/refresh`

**Refresh Token**

使用刷新令牌刷新访问令牌。

返回:
    新的访问令牌

**Request Body**:
```json
RefreshTokenRequest
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_TokenResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 用户管理

### User

#### POST `/api/v1/users`

**Create User**

创建用户

需要权限：sys:user:create

Args:
    user_data: 用户创建数据

Returns:
    创建的用户信息

**Request Body**:
```json
UserCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/users`

**Get Users**

获取用户列表（分页）

需要权限：sys:user:list

Args:
    name: 用户名（模糊搜索）
    real_name: 真实姓名（模糊搜索）
    email: 邮箱（模糊搜索）
    mobile: 联系电话（模糊搜索）
    status: 状态:1-Active,2-InActive
    is_superuser: 是否超级用户:0-普通,1-超级
    page: 页码
    page_size: 每页条目数

Returns:
    用户分页列表

**Query Parameters**:
- `name` (optional): string - 用户名（模糊搜索）
- `real_name` (optional): string - 真实姓名（模糊搜索）
- `email` (optional): string - 邮箱（模糊搜索）
- `mobile` (optional): string - 联系电话（模糊搜索）
- `status` (optional): string - 状态:1-Active,2-InActive
- `is_superuser` (optional): string - 是否超级用户:0-普通,1-超级
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_UserResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/users/me`

**Get Current User Info**

获取当前登录用户的个人信息

无需特殊权限，只需登录即可

Returns:
    当前用户信息

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResponse_
  ```

---

#### GET `/api/v1/users/me/menus`

**Get Current User Menus**

获取当前登录用户的菜单列表

无需特殊权限，只需登录即可

Returns:
    当前用户的菜单树形结构列表

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_list_MenuTreeNode__
  ```

---

#### GET `/api/v1/users/options`

**Get User Options**

获取用户选项列表（用于下拉框）

需要权限：sys:user:list

Args:
    status: 状态过滤（可选）

Returns:
    用户选项列表

**Query Parameters**:
- `status` (optional): string - 状态:1-Active,2-InActive

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_list_UserOption__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/users/{user_id}`

**Get User**

获取用户详情

需要权限：sys:user:view

Args:
    user_id: 用户ID

Returns:
    用户信息

**Path Parameters**:
- `user_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/users/{user_id}`

**Update User**

更新用户信息

需要权限：sys:user:update

Args:
    user_id: 用户ID
    user_data: 用户更新数据

Returns:
    更新后的用户信息

**Request Body**:
```json
UserUpdate
```

**Path Parameters**:
- `user_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/users/{user_id}`

**Delete User**

删除用户（软删除）

需要权限：sys:user:delete

Args:
    user_id: 用户ID

Returns:
    成功消息

**Path Parameters**:
- `user_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PATCH `/api/v1/users/{user_id}/password`

**Update User Password**

更新用户密码

需要权限：sys:user:update

Args:
    user_id: 用户ID
    password_data: 密码更新数据

Returns:
    成功消息

**Request Body**:
```json
UserPasswordUpdate
```

**Path Parameters**:
- `user_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PATCH `/api/v1/users/{user_id}/status`

**Update User Status**

更新用户状态

需要权限：sys:user:update

Args:
    user_id: 用户ID
    status_data: 状态更新数据

Returns:
    更新后的用户信息

**Request Body**:
```json
UserStatusUpdate
```

**Path Parameters**:
- `user_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 部门管理

### Department

#### POST `/api/v1/departments`

**Create Department**

创建部门

需要权限：sys:department:create

Args:
    department_data: 部门创建数据

Returns:
    创建的部门信息

**Request Body**:
```json
DepartmentCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_DepartmentResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/departments`

**Get Departments**

获取部门列表（分页）

需要权限：sys:department:list

Args:
    name: 部门名称（模糊搜索）
    code: 部门编码（模糊搜索）
    status: 状态:1-Active,2-InActive
    parent_id: 上级部门id
    page: 页码
    page_size: 每页条目数

Returns:
    部门分页列表

**Query Parameters**:
- `name` (optional): string - 部门名称（模糊搜索）
- `code` (optional): string - 部门编码（模糊搜索）
- `status` (optional): string - 状态:1-Active,2-InActive
- `parent_id` (optional): string - 上级部门id
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_DepartmentResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/departments/options`

**Get Department Options**

获取部门选项列表（用于下拉框）

需要权限：sys:department:list

Args:
    status: 状态过滤（可选）

Returns:
    部门选项列表

**Query Parameters**:
- `status` (optional): string - 状态:1-Active,2-InActive

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_list_DepartmentOption__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/departments/tree`

**Get Department Tree**

获取部门树形结构

需要权限：sys:department:list

Args:
    status: 状态过滤（可选）

Returns:
    部门树形结构

**Query Parameters**:
- `status` (optional): string - 状态:1-Active,2-InActive

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_list_DepartmentTreeNode__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/departments/{department_id}`

**Get Department**

获取部门详情

需要权限：sys:department:view

Args:
    department_id: 部门ID

Returns:
    部门信息

**Path Parameters**:
- `department_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_DepartmentResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/departments/{department_id}`

**Update Department**

更新部门信息

需要权限：sys:department:update

Args:
    department_id: 部门ID
    department_data: 部门更新数据

Returns:
    更新后的部门信息

**Request Body**:
```json
DepartmentUpdate
```

**Path Parameters**:
- `department_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_DepartmentResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/departments/{department_id}`

**Delete Department**

删除部门（软删除）

需要权限：sys:department:delete

Args:
    department_id: 部门ID

Returns:
    成功消息

**Path Parameters**:
- `department_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PATCH `/api/v1/departments/{department_id}/status`

**Update Department Status**

更新部门状态

需要权限：sys:department:update

Args:
    department_id: 部门ID
    status_data: 状态更新数据

Returns:
    更新后的部门信息

**Request Body**:
```json
DepartmentStatusUpdate
```

**Path Parameters**:
- `department_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_DepartmentResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 职务管理

### Position

#### POST `/api/v1/positions`

**Create Position**

创建职务

需要权限：sys:position:create

Args:
    position_data: 职务创建数据

Returns:
    创建的职务信息

**Request Body**:
```json
PositionCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PositionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/positions`

**Get Positions**

获取职务列表（分页）

需要权限：sys:position:list

Args:
    name: 职务名称（模糊搜索）
    code: 职务编码（模糊搜索）
    status: 状态:1-Active,2-InActive
    page: 页码
    page_size: 每页条目数

Returns:
    职务分页列表

**Query Parameters**:
- `name` (optional): string - 职务名称（模糊搜索）
- `code` (optional): string - 职务编码（模糊搜索）
- `status` (optional): string - 状态:1-Active,2-InActive
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_PositionResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/positions/options`

**Get Position Options**

获取职务选项列表（用于下拉框）

需要权限：sys:position:list

Args:
    status: 状态过滤（可选）

Returns:
    职务选项列表

**Query Parameters**:
- `status` (optional): string - 状态:1-Active,2-InActive

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_list_PositionOption__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/positions/{position_id}`

**Get Position**

获取职务详情

需要权限：sys:position:view

Args:
    position_id: 职务ID

Returns:
    职务信息

**Path Parameters**:
- `position_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PositionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/positions/{position_id}`

**Update Position**

更新职务信息

需要权限：sys:position:update

Args:
    position_id: 职务ID
    position_data: 职务更新数据

Returns:
    更新后的职务信息

**Request Body**:
```json
PositionUpdate
```

**Path Parameters**:
- `position_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PositionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/positions/{position_id}`

**Delete Position**

删除职务（软删除）

需要权限：sys:position:delete

Args:
    position_id: 职务ID

Returns:
    成功消息

**Path Parameters**:
- `position_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PATCH `/api/v1/positions/{position_id}/status`

**Update Position Status**

更新职务状态

需要权限：sys:position:update

Args:
    position_id: 职务ID
    status_data: 状态更新数据

Returns:
    更新后的职务信息

**Request Body**:
```json
PositionStatusUpdate
```

**Path Parameters**:
- `position_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PositionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 菜单集合

### MenuCollection

#### POST `/api/v1/menu-collections`

**Create Menu Collection**

创建菜单集合

需要权限：sys:menu-collection:create

Args:
    collection_data: 菜单集合创建数据

Returns:
    创建的菜单集合信息

**Request Body**:
```json
MenuCollectionCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_MenuCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/menu-collections`

**Get Menu Collections**

获取菜单集合列表（分页）

需要权限：sys:menu-collection:list

Args:
    name: 菜单集合名称（模糊搜索）
    status: 状态
    page: 页码
    page_size: 每页条目数

Returns:
    菜单集合分页列表

**Query Parameters**:
- `name` (optional): string - 菜单集合名称（模糊搜索）
- `status` (optional): string - 状态:1-Active,2-InActive
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_MenuCollectionResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/menu-collections/options`

**Get Menu Collection Options**

获取菜单集合选项列表（用于下拉框）

需要权限：sys:menu-collection:list

Args:
    status: 状态过滤

Returns:
    菜单集合选项列表

**Query Parameters**:
- `status` (optional): string - 状态:1-Active,2-InActive

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_list_MenuCollectionOption__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/menu-collections/{collection_id}`

**Get Menu Collection**

获取菜单集合详情

需要权限：sys:menu-collection:list

Args:
    collection_id: 菜单集合ID

Returns:
    菜单集合信息（包含菜单列表）

**Path Parameters**:
- `collection_id` (required): 菜单集合ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_MenuCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/menu-collections/{collection_id}`

**Update Menu Collection**

更新菜单集合信息

需要权限：sys:menu-collection:update

Args:
    collection_id: 菜单集合ID
    collection_data: 菜单集合更新数据

Returns:
    更新后的菜单集合信息

**Request Body**:
```json
MenuCollectionUpdate
```

**Path Parameters**:
- `collection_id` (required): 菜单集合ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_MenuCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/menu-collections/{collection_id}`

**Delete Menu Collection**

删除菜单集合（软删除）

需要权限：sys:menu-collection:delete

Args:
    collection_id: 菜单集合ID

Returns:
    成功消息

**Path Parameters**:
- `collection_id` (required): 菜单集合ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PATCH `/api/v1/menu-collections/{collection_id}/status`

**Update Menu Collection Status**

更新菜单集合状态

需要权限：sys:menu-collection:update

Args:
    collection_id: 菜单集合ID
    status_data: 状态更新数据

Returns:
    更新后的菜单集合信息

**Request Body**:
```json
MenuCollectionStatusUpdate
```

**Path Parameters**:
- `collection_id` (required): 菜单集合ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_MenuCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 菜单

### Menu

#### POST `/api/v1/menus`

**Create Menu**

创建菜单

需要权限：sys:menu:create

Args:
    menu_data: 菜单创建数据

Returns:
    创建的菜单信息

**Request Body**:
```json
MenuCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_MenuResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/menus`

**Get Menus**

获取菜单列表（分页）

需要权限：sys:menu:list

Args:
    name: 菜单名称（模糊搜索）
    title: 菜单显示名称（模糊搜索）
    status: 状态
    menu_type: 菜单类型
    parent_id: 父菜单id
    page: 页码
    page_size: 每页条目数

Returns:
    菜单分页列表

**Query Parameters**:
- `name` (optional): string - 菜单名称（模糊搜索）
- `title` (optional): string - 菜单显示名称（模糊搜索）
- `status` (optional): string - 状态:1-Active,2-InActive
- `menu_type` (optional): string - 菜单类型:0-目录,1-菜单,2-按钮
- `parent_id` (optional): string - 父菜单id
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_MenuResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/menus/options`

**Get Menu Options**

获取菜单选项列表（用于下拉框）

需要权限：sys:menu:list

Args:
    status: 状态过滤
    menu_type: 菜单类型过滤

Returns:
    菜单选项列表

**Query Parameters**:
- `status` (optional): string - 状态:1-Active,2-InActive
- `menu_type` (optional): string - 菜单类型:0-目录,1-菜单,2-按钮

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_list_MenuOption__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/menus/tree`

**Get Menu Tree**

获取菜单树形结构

需要权限：sys:menu:list

Args:
    status: 状态过滤
    menu_type: 菜单类型过滤

Returns:
    菜单树形结构列表

**Query Parameters**:
- `status` (optional): string - 状态:1-Active,2-InActive
- `menu_type` (optional): string - 菜单类型:0-目录,1-菜单,2-按钮

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_list_MenuTreeNode__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/menus/{menu_id}`

**Get Menu**

获取菜单详情

需要权限：sys:menu:list

Args:
    menu_id: 菜单ID

Returns:
    菜单信息

**Path Parameters**:
- `menu_id` (required): 菜单ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_MenuResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/menus/{menu_id}`

**Update Menu**

更新菜单信息

需要权限：sys:menu:update

Args:
    menu_id: 菜单ID
    menu_data: 菜单更新数据

Returns:
    更新后的菜单信息

**Request Body**:
```json
MenuUpdate
```

**Path Parameters**:
- `menu_id` (required): 菜单ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_MenuResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/menus/{menu_id}`

**Delete Menu**

删除菜单（软删除）

需要权限：sys:menu:delete

Args:
    menu_id: 菜单ID

Returns:
    成功消息

**Path Parameters**:
- `menu_id` (required): 菜单ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PATCH `/api/v1/menus/{menu_id}/status`

**Update Menu Status**

更新菜单状态

需要权限：sys:menu:update

Args:
    menu_id: 菜单ID
    status_data: 状态更新数据

Returns:
    更新后的菜单信息

**Request Body**:
```json
MenuStatusUpdate
```

**Path Parameters**:
- `menu_id` (required): 菜单ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_MenuResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 权限集合

### PermissionCollection

#### POST `/api/v1/permission-collections`

**Create Permission Collection**

创建权限集合

需要权限：sys:permission-collection:create

Args:
    data: 权限集合创建数据

Returns:
    创建的权限集合

**Request Body**:
```json
PermissionCollectionCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PermissionCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/permission-collections`

**Get Permission Collections**

获取权限集合列表（分页）

需要权限：sys:permission-collection:list

Args:
    name: 权限集合名称(模糊搜索)
    status: 状态
    page: 页码
    page_size: 每页条目数

Returns:
    权限集合分页列表

**Query Parameters**:
- `name` (optional): string - 权限集合名称(模糊搜索)
- `status` (optional): string - 状态
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_PermissionCollectionResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/permission-collections/options`

**Get Permission Collection Options**

获取权限集合选项列表（用于下拉选择）

需要权限：sys:permission-collection:list

Args:
    status: 状态过滤（可选）

Returns:
    权限集合选项列表

**Query Parameters**:
- `status` (optional): string - 状态

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_List_PermissionCollectionOptionsResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/permission-collections/{permission_collection_id}`

**Update Permission Collection**

更新权限集合

需要权限：sys:permission-collection:update

Args:
    permission_collection_id: 权限集合ID
    data: 权限集合更新数据

Returns:
    更新后的权限集合

**Request Body**:
```json
PermissionCollectionUpdate
```

**Path Parameters**:
- `permission_collection_id` (required): 权限集合ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PermissionCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/permission-collections/{permission_collection_id}`

**Delete Permission Collection**

删除权限集合（软删除）

需要权限：sys:permission-collection:delete

Args:
    permission_collection_id: 权限集合ID

Returns:
    成功消息

**Path Parameters**:
- `permission_collection_id` (required): 权限集合ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/permission-collections/{permission_collection_id}`

**Get Permission Collection**

获取权限集合详情

需要权限：sys:permission-collection:list

Args:
    permission_collection_id: 权限集合ID

Returns:
    权限集合详情

**Path Parameters**:
- `permission_collection_id` (required): 权限集合ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PermissionCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 权限

### Permission

#### POST `/api/v1/permissions`

**Create Permission**

创建权限

需要权限：sys:permission:create

Args:
    data: 权限创建数据

Returns:
    创建的权限

**Request Body**:
```json
PermissionCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PermissionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/permissions`

**Get Permissions**

获取权限列表（分页）

需要权限：sys:permission:list

Args:
    name: 权限名称(模糊搜索)
    code: 权限编码(模糊搜索)
    status: 状态
    page: 页码
    page_size: 每页条目数

Returns:
    权限分页列表

**Query Parameters**:
- `name` (optional): string - 权限名称(模糊搜索)
- `code` (optional): string - 权限编码(模糊搜索)
- `status` (optional): string - 状态
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_PermissionResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/permissions/options`

**Get Permission Options**

获取权限选项列表（用于下拉选择）

需要权限：sys:permission:list

Args:
    status: 状态过滤（可选）

Returns:
    权限选项列表

**Query Parameters**:
- `status` (optional): string - 状态

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_List_PermissionOptionsResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/permissions/{permission_id}`

**Update Permission**

更新权限

需要权限：sys:permission:update

Args:
    permission_id: 权限ID
    data: 权限更新数据

Returns:
    更新后的权限

**Request Body**:
```json
PermissionUpdate
```

**Path Parameters**:
- `permission_id` (required): 权限ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PermissionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/permissions/{permission_id}`

**Delete Permission**

删除权限（软删除）

需要权限：sys:permission:delete

Args:
    permission_id: 权限ID

Returns:
    成功消息

**Path Parameters**:
- `permission_id` (required): 权限ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/permissions/{permission_id}`

**Get Permission**

获取权限详情

需要权限：sys:permission:list

Args:
    permission_id: 权限ID

Returns:
    权限详情

**Path Parameters**:
- `permission_id` (required): 权限ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PermissionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 资源类型

### ResourceType

#### POST `/api/v1/resource-types`

**Create Resource Type**

创建资源类型

需要权限：sys:resource-type:create

Args:
    data: 资源类型创建数据

Returns:
    创建的资源类型

**Request Body**:
```json
ResourceTypeCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceTypeResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-types`

**Get Resource Types**

获取资源类型列表（分页）

需要权限：sys:resource-type:list

Args:
    name: 资源类型名称(模糊搜索)
    code: 资源类型编码(模糊搜索)
    status: 状态
    page: 页码
    page_size: 每页条目数

Returns:
    资源类型分页列表

**Query Parameters**:
- `name` (optional): string - 资源类型名称(模糊搜索)
- `code` (optional): string - 资源类型编码(模糊搜索)
- `status` (optional): string - 状态
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_ResourceTypeResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-types/options/list`

**Get Resource Type Options**

获取资源类型下拉选项列表

需要权限：sys:resource-type:list

Returns:
    资源类型选项列表

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_List_ResourceTypeOption__
  ```

---

#### PUT `/api/v1/resource-types/{resource_type_id}`

**Update Resource Type**

更新资源类型

需要权限：sys:resource-type:update

Args:
    resource_type_id: 资源类型ID
    data: 资源类型更新数据

Returns:
    更新后的资源类型

**Request Body**:
```json
ResourceTypeUpdate
```

**Path Parameters**:
- `resource_type_id` (required): 资源类型ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceTypeResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/resource-types/{resource_type_id}`

**Delete Resource Type**

删除资源类型（软删除）

需要权限：sys:resource-type:delete

Args:
    resource_type_id: 资源类型ID

Returns:
    成功消息

**Path Parameters**:
- `resource_type_id` (required): 资源类型ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-types/{resource_type_id}`

**Get Resource Type**

获取资源类型详情

需要权限：sys:resource-type:list

Args:
    resource_type_id: 资源类型ID

Returns:
    资源类型详情

**Path Parameters**:
- `resource_type_id` (required): 资源类型ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceTypeResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 资源类型规则

### ResourceTypeRule

#### POST `/api/v1/resource-type-rules`

**Create Resource Type Rule**

创建资源类型规则

需要权限：sys:resource-type-rule:create

Args:
    data: 资源类型规则创建数据

Returns:
    创建的资源类型规则

**Request Body**:
```json
ResourceTypeRuleCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceTypeRuleResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-type-rules`

**Get Resource Type Rules**

获取资源类型规则列表（分页）

需要权限：sys:resource-type-rule:list

Args:
    resource_type_id: 资源类型ID
    table_name: 业务表名(模糊搜索)
    field_name: 过滤字段名(模糊搜索)
    status: 状态
    page: 页码
    page_size: 每页条目数

Returns:
    资源类型规则分页列表

**Query Parameters**:
- `resource_type_id` (optional): string - 资源类型ID
- `table_name` (optional): string - 业务表名(模糊搜索)
- `field_name` (optional): string - 过滤字段名(模糊搜索)
- `status` (optional): string - 状态
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_ResourceTypeRuleWithDetailResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### POST `/api/v1/resource-type-rules/batch`

**Batch Create Resource Type Rule**

批量创建资源类型规则

需要权限：sys:resource-type-rule:create

Args:
    data: 批量创建数据

Returns:
    创建的规则数量

**Request Body**:
```json
ResourceTypeRuleBatchCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_int_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-type-rules/by-resource-type/{resource_type_id}`

**Get Rules By Resource Type**

根据资源类型ID获取规则列表

需要权限：sys:resource-type-rule:list

Args:
    resource_type_id: 资源类型ID

Returns:
    规则列表

**Path Parameters**:
- `resource_type_id` (required): 资源类型ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_List_ResourceTypeRuleResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/resource-type-rules/{rule_id}`

**Update Resource Type Rule**

更新资源类型规则

需要权限：sys:resource-type-rule:update

Args:
    rule_id: 资源类型规则ID
    data: 资源类型规则更新数据

Returns:
    更新后的资源类型规则

**Request Body**:
```json
ResourceTypeRuleUpdate
```

**Path Parameters**:
- `rule_id` (required): 资源类型规则ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceTypeRuleResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/resource-type-rules/{rule_id}`

**Delete Resource Type Rule**

删除资源类型规则（软删除）

需要权限：sys:resource-type-rule:delete

Args:
    rule_id: 资源类型规则ID

Returns:
    成功消息

**Path Parameters**:
- `rule_id` (required): 资源类型规则ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-type-rules/{rule_id}`

**Get Resource Type Rule**

获取资源类型规则详情

需要权限：sys:resource-type-rule:list

Args:
    rule_id: 资源类型规则ID

Returns:
    资源类型规则详情

**Path Parameters**:
- `rule_id` (required): 资源类型规则ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceTypeRuleResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 资源范围

### ResourceScope

#### POST `/api/v1/resource-scopes`

**Create Resource Scope**

创建资源范围

需要权限：sys:resource-scope:create

Args:
    data: 资源范围创建数据

Returns:
    创建的资源范围

**Request Body**:
```json
ResourceScopeCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceScopeResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-scopes`

**Get Resource Scopes**

获取资源范围列表（分页）

需要权限：sys:resource-scope:list

Args:
    name: 资源范围名称(模糊搜索)
    resource_type_id: 资源类型ID
    status: 状态
    page: 页码
    page_size: 每页条目数

Returns:
    资源范围分页列表

**Query Parameters**:
- `name` (optional): string - 资源范围名称(模糊搜索)
- `resource_type_id` (optional): string - 资源类型ID
- `status` (optional): string - 状态
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_ResourceScopeWithDetailResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-scopes/by-resource-type/{resource_type_id}`

**Get Scopes By Resource Type**

根据资源类型ID获取资源范围列表

需要权限：sys:resource-scope:list

Args:
    resource_type_id: 资源类型ID

Returns:
    资源范围列表

**Path Parameters**:
- `resource_type_id` (required): 资源类型ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_List_ResourceScopeResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-scopes/options/list`

**Get Resource Scope Options**

获取资源范围下拉选项列表

需要权限：sys:resource-scope:list

Args:
    resource_type_id: 资源类型ID（可选，用于过滤）

Returns:
    资源范围选项列表

**Query Parameters**:
- `resource_type_id` (optional): string - 资源类型ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_List_ResourceScopeOption__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/resource-scopes/{resource_scope_id}`

**Update Resource Scope**

更新资源范围

需要权限：sys:resource-scope:update

Args:
    resource_scope_id: 资源范围ID
    data: 资源范围更新数据

Returns:
    更新后的资源范围

**Request Body**:
```json
ResourceScopeUpdate
```

**Path Parameters**:
- `resource_scope_id` (required): 资源范围ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceScopeResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/resource-scopes/{resource_scope_id}`

**Delete Resource Scope**

删除资源范围（软删除）

需要权限：sys:resource-scope:delete

Args:
    resource_scope_id: 资源范围ID

Returns:
    成功消息

**Path Parameters**:
- `resource_scope_id` (required): 资源范围ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-scopes/{resource_scope_id}`

**Get Resource Scope**

获取资源范围详情

需要权限：sys:resource-scope:list

Args:
    resource_scope_id: 资源范围ID

Returns:
    资源范围详情

**Path Parameters**:
- `resource_scope_id` (required): 资源范围ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceScopeResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 资源范围详情

### ResourceScopeDetail

#### POST `/api/v1/resource-scope-details`

**Create Resource Scope Detail**

创建资源范围明细

需要权限：sys:resource-scope-detail:create

Args:
    data: 资源范围明细创建数据

Returns:
    创建的资源范围明细

**Request Body**:
```json
ResourceScopeDetailCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceScopeDetailResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-scope-details`

**Get Resource Scope Details**

获取资源范围明细列表（分页）

需要权限：sys:resource-scope-detail:list

Args:
    resource_scope_id: 资源范围ID
    resource_id: 资源ID
    access_type: 访问类型
    page: 页码
    page_size: 每页条目数

Returns:
    资源范围明细分页列表

**Query Parameters**:
- `resource_scope_id` (optional): string - 资源范围ID
- `resource_id` (optional): string - 资源ID
- `access_type` (optional): string - 访问类型
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_ResourceScopeDetailWithInfoResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### POST `/api/v1/resource-scope-details/batch`

**Batch Create Resource Scope Detail**

批量创建资源范围明细

需要权限：sys:resource-scope-detail:create

Args:
    data: 批量创建数据

Returns:
    创建的明细数量

**Request Body**:
```json
ResourceScopeDetailBatchCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_int_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-scope-details/by-scope/{resource_scope_id}`

**Get Details By Scope**

根据资源范围ID获取明细列表

需要权限：sys:resource-scope-detail:list

Args:
    resource_scope_id: 资源范围ID

Returns:
    明细列表

**Path Parameters**:
- `resource_scope_id` (required): 资源范围ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_List_ResourceScopeDetailResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/resource-scope-details/{detail_id}`

**Update Resource Scope Detail**

更新资源范围明细

需要权限：sys:resource-scope-detail:update

Args:
    detail_id: 资源范围明细ID
    data: 资源范围明细更新数据

Returns:
    更新后的资源范围明细

**Request Body**:
```json
ResourceScopeDetailUpdate
```

**Path Parameters**:
- `detail_id` (required): 资源范围明细ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceScopeDetailResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/resource-scope-details/{detail_id}`

**Delete Resource Scope Detail**

删除资源范围明细（软删除）

需要权限：sys:resource-scope-detail:delete

Args:
    detail_id: 资源范围明细ID

Returns:
    成功消息

**Path Parameters**:
- `detail_id` (required): 资源范围明细ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-scope-details/{detail_id}`

**Get Resource Scope Detail**

获取资源范围明细详情

需要权限：sys:resource-scope-detail:list

Args:
    detail_id: 资源范围明细ID

Returns:
    资源范围明细详情

**Path Parameters**:
- `detail_id` (required): 资源范围明细ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceScopeDetailResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 资源协作范围

### ResourceCollaborationScope

#### POST `/api/v1/resource-collaboration-scopes`

**Create Resource Collaboration Scope**

创建资源协作范围关系

需要权限：sys:resource-collaboration-scope:create

Args:
    data: 资源协作范围关系创建数据

Returns:
    创建的资源协作范围关系

**Request Body**:
```json
ResourceCollaborationScopeCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_ResourceCollaborationScopeResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/resource-collaboration-scopes`

**Get Resource Collaboration Scope Relations**

获取资源协作范围关系列表（分页）

需要权限：sys:resource-collaboration-scope:list

Args:
    collaboration_id: 资源协作ID
    resource_scope_id: 资源范围ID
    page: 页码
    page_size: 每页条目数

Returns:
    资源协作范围关系分页列表

**Query Parameters**:
- `collaboration_id` (optional): string - 资源协作ID
- `resource_scope_id` (optional): string - 资源范围ID
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_ResourceCollaborationScopeWithDetailResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/resource-collaboration-scopes`

**Delete Resource Collaboration Scope**

删除资源协作范围关系

需要权限：sys:resource-collaboration-scope:delete

Args:
    collaboration_id: 资源协作ID
    resource_scope_id: 资源范围ID

Returns:
    成功消息

**Query Parameters**:
- `collaboration_id` (required): integer - 资源协作ID
- `resource_scope_id` (required): integer - 资源范围ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### POST `/api/v1/resource-collaboration-scopes/batch`

**Batch Create Resource Collaboration Scope**

批量创建资源协作范围关系

需要权限：sys:resource-collaboration-scope:create

Args:
    data: 批量创建数据

Returns:
    创建的关系数量

**Request Body**:
```json
ResourceCollaborationScopeBatchCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_int_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 用户菜单

### UserMenu

#### POST `/api/v1/user-menus/users/menus`

**Set User Menus**

为用户设置菜单（先删除旧关系，再创建新关系）

需要权限：sys:user-menu:update

Args:
    data: 设置数据（user_id, menu_ids）

Returns:
    创建的关系数量

**Request Body**:
```json
UserMenuSetRequest
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_int_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/user-menus/users/{user_id}/menus`

**Get User Menus**

获取用户的菜单列表

需要权限：sys:user-menu:list

Args:
    user_id: 用户ID

Returns:
    用户菜单响应（包含菜单详细信息）

**Path Parameters**:
- `user_id` (required): 用户ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserMenuResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 用户菜单集合

### UserMenuCollection

#### POST `/api/v1/user-menu-collections/users/menu-collections`

**Set User Menu Collections**

为用户设置菜单集合（先删除旧关系，再创建新关系）

需要权限：sys:user-menu-collection:update

Args:
    data: 设置数据（user_id, menu_collection_ids）

Returns:
    创建的关系数量

**Request Body**:
```json
UserMenuCollectionSetRequest
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_int_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/user-menu-collections/users/{user_id}/menu-collections`

**Get User Menu Collections**

获取用户的菜单集合列表

需要权限：sys:user-menu-collection:list

Args:
    user_id: 用户ID

Returns:
    用户菜单集合响应（包含菜单集合详细信息）

**Path Parameters**:
- `user_id` (required): 用户ID

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserMenuCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 用户权限

### UserPermission

#### POST `/api/v1/user-permissions/users/permissions`

**Set User Permissions**

为用户设置权限（先删除旧关系，再创建新关系）

需要权限：sys:user-permission:update

Args:
    data: 设置数据（user_id, permission_ids, policy）

Returns:
    创建的关系数量

**Request Body**:
```json
UserPermissionSetRequest
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_int_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/user-permissions/users/{user_id}/permissions`

**Get User Permissions**

获取用户的权限列表

需要权限：sys:user-permission:list

Args:
    user_id: 用户ID
    policy: 策略（1-allow, 2-deny）

Returns:
    用户权限响应（包含权限详细信息）

**Path Parameters**:
- `user_id` (required): 用户ID

**Query Parameters**:
- `policy` (required): integer - 策略:1-allow,2-deny

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserPermissionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 用户权限集合

### UserPermissionCollection

#### POST `/api/v1/user-permission-collections/users/permission-collections`

**Set User Permission Collections**

为用户设置权限集合（先删除旧关系，再创建新关系）

需要权限：sys:user-permission-collection:update

Args:
    data: 设置数据（user_id, permission_collection_ids, policy）

Returns:
    创建的关系数量

**Request Body**:
```json
UserPermissionCollectionSetRequest
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_int_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/user-permission-collections/users/{user_id}/permission-collections`

**Get User Permission Collections**

获取用户的权限集合列表

需要权限：sys:user-permission-collection:list

Args:
    user_id: 用户ID
    policy: 策略（1-allow, 2-deny）

Returns:
    用户权限集合响应（包含权限集合详细信息）

**Path Parameters**:
- `user_id` (required): 用户ID

**Query Parameters**:
- `policy` (required): integer - 策略:1-allow,2-deny

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserPermissionCollectionResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 用户资源范围

### UserResourceScope

#### POST `/api/v1/user-resource-scopes`

**Create User Resource Scope**

创建用户资源范围关系

需要权限：sys:user-resource-scope:create

Args:
    data: 用户资源范围关系创建数据

Returns:
    创建的用户资源范围关系

**Request Body**:
```json
UserResourceScopeCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResourceScopeResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/user-resource-scopes`

**Get User Resource Scope Relations**

获取用户资源范围关系列表（分页）

需要权限：sys:user-resource-scope:list

Args:
    user_id: 用户ID
    resource_scope_id: 资源范围ID
    page: 页码
    page_size: 每页条目数

Returns:
    用户资源范围关系分页列表

**Query Parameters**:
- `user_id` (optional): string - 用户ID
- `resource_scope_id` (optional): string - 资源范围ID
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_UserResourceScopeWithDetailResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/user-resource-scopes`

**Delete User Resource Scope**

删除用户资源范围关系

需要权限：sys:user-resource-scope:delete

Args:
    user_id: 用户ID
    resource_scope_id: 资源范围ID

Returns:
    成功消息

**Query Parameters**:
- `user_id` (required): integer - 用户ID
- `resource_scope_id` (required): integer - 资源范围ID

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### POST `/api/v1/user-resource-scopes/batch`

**Batch Create User Resource Scope**

批量创建用户资源范围关系

需要权限：sys:user-resource-scope:create

Args:
    data: 批量创建数据

Returns:
    创建的关系数量

**Request Body**:
```json
UserResourceScopeBatchCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_int_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 用户资源协作

### UserResourceCollaboration

#### POST `/api/v1/user-resource-collaborations`

**Create User Resource Collaboration**

创建用户资源协作

需要权限：sys:user-resource-collaboration:create

Args:
    data: 用户资源协作创建数据

Returns:
    创建的用户资源协作

**Request Body**:
```json
UserResourceCollaborationCreate
```

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResourceCollaborationResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/user-resource-collaborations`

**Get User Resource Collaborations**

获取用户资源协作列表（分页）

需要权限：sys:user-resource-collaboration:list

Args:
    granter_id: 授权人ID
    grantee_id: 被授权人ID
    status: 状态
    access_type: 协作类型
    page: 页码
    page_size: 每页条目数

Returns:
    用户资源协作分页列表

**Query Parameters**:
- `granter_id` (optional): string - 授权人ID
- `grantee_id` (optional): string - 被授权人ID
- `status` (optional): string - 状态
- `access_type` (optional): string - 协作类型
- `page` (optional): integer - 页码
- `page_size` (optional): integer - 每页条目数

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_PaginatedResponse_UserResourceCollaborationWithDetailResponse__
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### PUT `/api/v1/user-resource-collaborations/{collaboration_id}`

**Update User Resource Collaboration**

更新用户资源协作

需要权限：sys:user-resource-collaboration:update

Args:
    collaboration_id: 协作ID
    data: 更新数据

Returns:
    更新后的用户资源协作

**Request Body**:
```json
UserResourceCollaborationUpdate
```

**Path Parameters**:
- `collaboration_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResourceCollaborationResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### GET `/api/v1/user-resource-collaborations/{collaboration_id}`

**Get User Resource Collaboration**

获取单个用户资源协作

需要权限：sys:user-resource-collaboration:read

Args:
    collaboration_id: 协作ID

Returns:
    用户资源协作

**Path Parameters**:
- `collaboration_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
ApiResponse_UserResourceCollaborationResponse_
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

#### DELETE `/api/v1/user-resource-collaborations/{collaboration_id}`

**Delete User Resource Collaboration**

删除用户资源协作

需要权限：sys:user-resource-collaboration:delete

Args:
    collaboration_id: 协作ID

Returns:
    成功消息

**Path Parameters**:
- `collaboration_id` (required): integer

**Responses**:

- `200`: Successful Response
  ```json
object
  ```
- `422`: Validation Error
  ```json
HTTPValidationError
  ```

---

## 数据模型 (Schemas)

### ApiResponse_DepartmentResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_List_PermissionCollectionOptionsResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_List_PermissionOptionsResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_List_ResourceScopeDetailResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_List_ResourceScopeOption__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_List_ResourceScopeResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_List_ResourceTypeOption__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_List_ResourceTypeRuleResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_MenuCollectionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_MenuResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_DepartmentResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_MenuCollectionResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_MenuResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_PermissionCollectionResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_PermissionResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_PositionResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_ResourceCollaborationScopeWithDetailResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_ResourceScopeDetailWithInfoResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_ResourceScopeWithDetailResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_ResourceTypeResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_ResourceTypeRuleWithDetailResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_UserResourceCollaborationWithDetailResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_UserResourceScopeWithDetailResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PaginatedResponse_UserResponse__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PermissionCollectionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PermissionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_PositionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_ResourceCollaborationScopeResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_ResourceScopeDetailResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_ResourceScopeResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_ResourceTypeResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_ResourceTypeRuleResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_TokenResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_UserMenuCollectionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_UserMenuResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_UserPermissionCollectionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_UserPermissionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_UserResourceCollaborationResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_UserResourceScopeResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_UserResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_int_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_list_DepartmentOption__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_list_DepartmentTreeNode__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_list_MenuCollectionOption__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_list_MenuOption__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_list_MenuTreeNode__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_list_PositionOption__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### ApiResponse_list_UserOption__

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| code | integer | * | 响应代码：0 表示成功，非 0 表示错误 |
| message | object |  | 响应消息 |
| data | object |  | 响应数据 |
| meta | object | * | 详细信息 |

### DepartmentCreate

创建部门请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | string | * | 部门名称 |
| code | string | * | 部门编码 |
| status | integer |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |
| parent_id | object |  | 上级部门id，None表示顶级部门 |
| leader_id | object |  | 部门负责人id |
| sort_flag | integer |  | 排序标志 |
| user_ids | object |  | 用户ID列表 |

### DepartmentOption

部门选项（用于下拉框）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer |  | ID |
| name | string |  | 名称 |
| status | integer |  | 状态 |
| status_name | string |  | 状态名称 |
| code | string |  | 部门编码 |
| parent_id | integer |  | 上级部门id |

### DepartmentResponse

部门响应

### DepartmentStatusUpdate

更新部门状态请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| status | integer | * | 状态:1-Active,2-InActive |

### DepartmentTreeNode

部门树形节点

### DepartmentUpdate

更新部门请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | object |  | 部门名称 |
| status | object |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |
| parent_id | object |  | 上级部门id，None表示顶级部门 |
| leader_id | object |  | 部门负责人id |
| sort_flag | object |  | 排序标志 |
| user_ids | object |  | 用户ID列表 |

### HTTPValidationError

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| detail | array<ValidationError> |  |  |

### LoginRequest

Login request schema.

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| username | string | * | Username |
| password | string | * | Password |

### MenuCollectionCreate

创建菜单集合请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | string | * | 菜单集合名称 |
| status | integer |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |
| menu_ids | object |  | 菜单ID列表 |

### MenuCollectionOption

菜单集合选项（用于下拉框）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer |  | ID |
| name | string |  | 名称 |
| status | integer |  | 状态 |
| status_name | string |  | 状态名称 |

### MenuCollectionResponse

菜单集合响应

### MenuCollectionSimpleResponse

菜单集合简单响应（用于嵌套在其他模型中）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 菜单集合ID |
| name | string | * | 菜单集合名称 |
| status | integer | * | 状态:1-Active,2-InActive |
| description | string |  | 描述信息 |

### MenuCollectionStatusUpdate

更新菜单集合状态请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| status | integer | * | 状态:1-Active,2-InActive |

### MenuCollectionUpdate

更新菜单集合请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | object |  | 菜单集合名称 |
| status | object |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |
| menu_ids | object |  | 菜单ID列表 |

### MenuCreate

创建菜单请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| parent_id | integer |  | 父菜单id，0表示顶级菜单 |
| name | string | * | 菜单名称 |
| title | string | * | 菜单前端显示名称 |
| path | object |  | 菜单前端路径 |
| icon | object |  | 菜单图标 |
| target | integer |  | 打开方式:1-当前窗口,2-新窗口 |
| menu_type | integer |  | 菜单类型:0-目录,1-菜单,2-按钮 |
| sort_flag | integer |  | 排序标志 |
| status | integer |  | 状态:1-Active,2-InActive |
| menu_collection_ids | object |  | 菜单集合ID列表 |

### MenuOption

菜单选项（用于下拉框）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer |  | ID |
| name | string |  | 名称 |
| status | integer |  | 状态 |
| status_name | string |  | 状态名称 |
| parent_id | integer |  | 父菜单id |
| menu_type | integer |  | 菜单类型 |
| menu_type_name | string |  | 菜单类型名称 |

### MenuResponse

菜单响应

### MenuStatusUpdate

更新菜单状态请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| status | integer | * | 状态:1-Active,2-InActive |

### MenuTreeNode

菜单树形节点

### MenuUpdate

更新菜单请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| parent_id | object |  | 父菜单id，0表示顶级菜单 |
| name | object |  | 菜单名称 |
| title | object |  | 菜单前端显示名称 |
| path | object |  | 菜单前端路径 |
| icon | object |  | 菜单图标 |
| target | object |  | 打开方式:1-当前窗口,2-新窗口 |
| menu_type | object |  | 菜单类型:0-目录,1-菜单,2-按钮 |
| sort_flag | object |  | 排序标志 |
| status | object |  | 状态:1-Active,2-InActive |
| menu_collection_ids | object |  | 菜单集合ID列表 |

### PaginatedResponse_DepartmentResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<DepartmentResponse> | * | 条目列表 |

### PaginatedResponse_MenuCollectionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<MenuCollectionResponse> | * | 条目列表 |

### PaginatedResponse_MenuResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<MenuResponse> | * | 条目列表 |

### PaginatedResponse_PermissionCollectionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<PermissionCollectionResponse> | * | 条目列表 |

### PaginatedResponse_PermissionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<PermissionResponse> | * | 条目列表 |

### PaginatedResponse_PositionResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<PositionResponse> | * | 条目列表 |

### PaginatedResponse_ResourceCollaborationScopeWithDetailResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<ResourceCollaborationScopeWithDetailResponse> | * | 条目列表 |

### PaginatedResponse_ResourceScopeDetailWithInfoResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<ResourceScopeDetailWithInfoResponse> | * | 条目列表 |

### PaginatedResponse_ResourceScopeWithDetailResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<ResourceScopeWithDetailResponse> | * | 条目列表 |

### PaginatedResponse_ResourceTypeResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<ResourceTypeResponse> | * | 条目列表 |

### PaginatedResponse_ResourceTypeRuleWithDetailResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<ResourceTypeRuleWithDetailResponse> | * | 条目列表 |

### PaginatedResponse_UserResourceCollaborationWithDetailResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<UserResourceCollaborationWithDetailResponse> | * | 条目列表 |

### PaginatedResponse_UserResourceScopeWithDetailResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<UserResourceScopeWithDetailResponse> | * | 条目列表 |

### PaginatedResponse_UserResponse_

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| total | integer | * | 总条目数 |
| page | integer | * | 当前页码 |
| page_size | integer | * | 每页条目数 |
| items | array<UserResponse> | * | 条目列表 |

### PermissionCollectionCreate

创建权限集合请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | string | * | 权限集合名称 |
| status | integer |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |
| permission_ids | object |  | 权限ID列表 |

### PermissionCollectionOptionsResponse

权限集合选项响应（用于下拉选择） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 权限集合ID |
| name | string | * | 权限集合名称 |
| status | integer | * | 状态:1-Active,2-InActive |

### PermissionCollectionResponse

权限集合响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 权限集合ID |
| name | string | * | 权限集合名称 |
| status | integer | * | 状态:1-Active,2-InActive |
| description | string |  | 描述信息 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |
| permissions | array<PermissionSimpleResponse> |  | 拥有的权限列表 |

### PermissionCollectionSimpleResponse

权限集合简化响应（用于Permission响应中） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 权限集合ID |
| name | string | * | 权限集合名称 |
| status | integer | * | 状态:1-Active,2-InActive |

### PermissionCollectionUpdate

更新权限集合请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | object |  | 权限集合名称 |
| status | object |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |
| permission_ids | object |  | 权限ID列表 |

### PermissionCreate

创建权限请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | string | * | 权限名称 |
| code | string | * | 权限编码 |
| api_path | object |  | API路径 |
| method | object |  | 请求方法 |
| status | integer |  | 状态:1-Active,2-InActive |
| permission_collection_ids | object |  | 权限集合ID列表 |

### PermissionOptionsResponse

权限选项响应（用于下拉选择） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 权限ID |
| name | string | * | 权限名称 |
| code | string | * | 权限编码 |

### PermissionResponse

权限响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 权限ID |
| name | string | * | 权限名称 |
| code | string | * | 权限编码 |
| api_path | string |  | API路径 |
| method | string |  | 请求方法 |
| status | integer | * | 状态:1-Active,2-InActive |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |
| permission_collections | array<PermissionCollectionSimpleResponse> |  | 所属权限集合列表 |

### PermissionSimpleResponse

权限简化响应（用于PermissionCollection响应中） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 权限ID |
| name | string | * | 权限名称 |
| code | string | * | 权限编码 |
| status | integer | * | 状态:1-Active,2-InActive |

### PermissionUpdate

更新权限请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | object |  | 权限名称 |
| code | object |  | 权限编码 |
| api_path | object |  | API路径 |
| method | object |  | 请求方法 |
| status | object |  | 状态:1-Active,2-InActive |
| permission_collection_ids | object |  | 权限集合ID列表 |

### PositionCreate

创建职务请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | string | * | 职务名称 |
| code | string | * | 职务编码 |
| status | integer |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |
| sort_flag | integer |  | 排序标志 |
| user_ids | object |  | 用户ID列表 |

### PositionOption

职务选项（用于下拉框）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer |  | ID |
| name | string |  | 名称 |
| status | integer |  | 状态 |
| status_name | string |  | 状态名称 |
| code | string |  | 职务编码 |

### PositionResponse

职务响应

### PositionStatusUpdate

更新职务状态请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| status | integer | * | 状态:1-Active,2-InActive |

### PositionUpdate

更新职务请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | object |  | 职务名称 |
| status | object |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |
| sort_flag | object |  | 排序标志 |
| user_ids | object |  | 用户ID列表 |

### RefreshTokenRequest

Refresh token request schema.

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| refresh_token | string | * | Refresh token |

### ResourceCollaborationScopeBatchCreate

批量创建资源协作范围关系请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| collaboration_id | integer | * | 资源协作ID |
| resource_scope_ids | array<integer> | * | 资源范围ID列表 |

### ResourceCollaborationScopeCreate

创建资源协作范围关系请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| collaboration_id | integer | * | 资源协作ID |
| resource_scope_id | integer | * | 资源范围ID |

### ResourceCollaborationScopeResponse

资源协作范围关系响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 关系ID |
| collaboration_id | integer | * | 资源协作ID |
| resource_scope_id | integer | * | 资源范围ID |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceCollaborationScopeWithDetailResponse

资源协作范围关系响应（包含详细信息） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 关系ID |
| collaboration_id | integer | * | 资源协作ID |
| granter_id | integer |  | 授权人ID |
| granter_name | string |  | 授权人用户名 |
| grantee_id | integer |  | 被授权人ID |
| grantee_name | string |  | 被授权人用户名 |
| resource_scope_id | integer | * | 资源范围ID |
| resource_scope_name | string |  | 资源范围名称 |
| resource_scope_status | integer |  | 资源范围状态 |
| resource_type_id | integer |  | 资源类型ID |
| resource_type_name | string |  | 资源类型名称 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceScopeCreate

创建资源范围请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | string | * | 资源范围名称 |
| resource_type_id | integer | * | 资源类型ID |
| status | integer |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |

### ResourceScopeDetailBatchCreate

批量创建资源范围明细请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| resource_scope_id | integer | * | 资源范围ID |
| details | array<object> | * | 明细列表 |

### ResourceScopeDetailCreate

创建资源范围明细请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| resource_scope_id | integer | * | 资源范围ID |
| resource_id | integer | * | 资源ID |
| access_type | integer | * | 访问类型:1-只读,2-读写 |

### ResourceScopeDetailResponse

资源范围明细响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 资源范围明细ID |
| resource_scope_id | integer | * | 资源范围ID |
| resource_id | integer | * | 资源ID |
| access_type | integer | * | 访问类型:1-只读,2-读写 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceScopeDetailUpdate

更新资源范围明细请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| resource_scope_id | object |  | 资源范围ID |
| resource_id | object |  | 资源ID |
| access_type | object |  | 访问类型:1-只读,2-读写 |

### ResourceScopeDetailWithInfoResponse

资源范围明细响应（包含详细信息） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 资源范围明细ID |
| resource_scope_id | integer | * | 资源范围ID |
| resource_scope_name | string |  | 资源范围名称 |
| resource_id | integer | * | 资源ID |
| access_type | integer | * | 访问类型:1-只读,2-读写 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceScopeOption

资源范围下拉选项 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer |  | ID |
| name | string |  | 名称 |
| status | integer |  | 状态 |
| status_name | string |  | 状态名称 |
| resource_type_id | integer |  | 资源类型ID |

### ResourceScopeResponse

资源范围响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 资源范围ID |
| name | string | * | 资源范围名称 |
| resource_type_id | integer | * | 资源类型ID |
| status | integer | * | 状态:1-Active,2-InActive |
| description | string |  | 描述信息 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceScopeUpdate

更新资源范围请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | object |  | 资源范围名称 |
| resource_type_id | object |  | 资源类型ID |
| status | object |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |

### ResourceScopeWithDetailResponse

资源范围响应（包含详细信息） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 资源范围ID |
| name | string | * | 资源范围名称 |
| resource_type_id | integer | * | 资源类型ID |
| resource_type_name | string |  | 资源类型名称 |
| resource_type_code | string |  | 资源类型编码 |
| status | integer | * | 状态:1-Active,2-InActive |
| description | string |  | 描述信息 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceTypeCreate

创建资源类型请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | string | * | 资源类型名称 |
| code | string | * | 资源类型编码 |
| status | integer |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |

### ResourceTypeOption

资源类型下拉选项 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer |  | ID |
| name | string |  | 名称 |
| status | integer |  | 状态 |
| status_name | string |  | 状态名称 |
| code | string |  | 资源类型编码 |

### ResourceTypeResponse

资源类型响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 资源类型ID |
| name | string | * | 资源类型名称 |
| code | string | * | 资源类型编码 |
| status | integer | * | 状态:1-Active,2-InActive |
| description | string |  | 描述信息 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceTypeRuleBatchCreate

批量创建资源类型规则请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| resource_type_id | integer | * | 资源类型ID |
| rules | array<object> | * | 规则列表 |

### ResourceTypeRuleCreate

创建资源类型规则请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| resource_type_id | integer | * | 资源类型ID |
| table_name | string | * | 业务表名 |
| field_name | string | * | 过滤字段名 |
| status | integer |  | 状态:1-Active,2-InActive |

### ResourceTypeRuleResponse

资源类型规则响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 资源类型规则ID |
| resource_type_id | integer | * | 资源类型ID |
| table_name | string | * | 业务表名 |
| field_name | string | * | 过滤字段名 |
| status | integer | * | 状态:1-Active,2-InActive |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceTypeRuleUpdate

更新资源类型规则请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| resource_type_id | object |  | 资源类型ID |
| table_name | object |  | 业务表名 |
| field_name | object |  | 过滤字段名 |
| status | object |  | 状态:1-Active,2-InActive |

### ResourceTypeRuleWithDetailResponse

资源类型规则响应（包含详细信息） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 资源类型规则ID |
| resource_type_id | integer | * | 资源类型ID |
| resource_type_name | string |  | 资源类型名称 |
| resource_type_code | string |  | 资源类型编码 |
| table_name | string | * | 业务表名 |
| field_name | string | * | 过滤字段名 |
| status | integer | * | 状态:1-Active,2-InActive |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### ResourceTypeUpdate

更新资源类型请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | object |  | 资源类型名称 |
| code | object |  | 资源类型编码 |
| status | object |  | 状态:1-Active,2-InActive |
| description | object |  | 描述信息 |

### TokenResponse

Token response schema.

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| access_token | string | * | Access token |
| refresh_token | string | * | Refresh token |
| token_type | string |  | Token type |
| expires_in | integer | * | Access token expiration time in seconds |

### UserBasicInfo

用户基本信息

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 用户ID |
| name | string | * | 用户名 |
| email | string |  | 邮箱 |

### UserCreate

创建用户请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| name | string | * | 用户名 |
| password | string | * | 密码 |
| real_name | object |  | 真实姓名 |
| email | object |  | 邮箱 |
| mobile | object |  | 联系电话 |
| status | integer |  | 状态:1-Active,2-InActive |
| is_superuser | integer |  | 是否超级用户:0-普通,1-超级 |
| department_ids | object |  | 部门ID列表 |
| position_ids | object |  | 职务ID列表 |

### UserMenuCollectionResponse

用户菜单集合响应（包含菜单集合详细信息）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| menu_collections | array<MenuCollectionResponse> |  | 菜单集合列表 |

### UserMenuCollectionSetRequest

设置用户菜单集合请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| menu_collection_ids | array<integer> | * | 菜单集合ID列表 |

### UserMenuResponse

用户菜单响应（包含菜单详细信息）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| menus | array<MenuResponse> |  | 菜单列表 |

### UserMenuSetRequest

设置用户菜单请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| menu_ids | array<integer> | * | 菜单ID列表 |

### UserOption

用户选项（用于下拉框）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer |  | ID |
| name | string |  | 名称 |
| status | integer |  | 状态 |
| status_name | string |  | 状态名称 |
| real_name | string |  | 真实姓名 |

### UserPasswordUpdate

更新用户密码请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| old_password | string | * | 旧密码 |
| new_password | string | * | 新密码 |

### UserPermissionCollectionResponse

用户权限集合响应（包含权限集合详细信息）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| policy | integer | * | 策略:1-allow,2-deny |
| permission_collections | array<PermissionCollectionResponse> |  | 权限集合列表 |

### UserPermissionCollectionSetRequest

设置用户权限集合请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| permission_collection_ids | array<integer> | * | 权限集合ID列表 |
| policy | integer | * | 策略:1-allow,2-deny |

### UserPermissionResponse

用户权限响应（包含权限详细信息）

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| policy | integer | * | 策略:1-allow,2-deny |
| permissions | array<PermissionResponse> |  | 权限列表 |

### UserPermissionSetRequest

设置用户权限请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| permission_ids | array<integer> | * | 权限ID列表 |
| policy | integer | * | 策略:1-allow,2-deny |

### UserResourceCollaborationCreate

创建用户资源协作请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| granter_id | integer | * | 授权人ID |
| grantee_id | integer | * | 被授权人ID |
| status | integer |  | 状态:1-Active,2-InActive |
| access_type | integer |  | 协作类型:0-跟随data_scope,1-只读,2-读写 |
| expire_at | object |  | 过期时间 |

### UserResourceCollaborationResponse

用户资源协作响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 协作ID |
| granter_id | integer | * | 授权人ID |
| grantee_id | integer | * | 被授权人ID |
| status | integer | * | 状态:1-Active,2-InActive |
| access_type | integer | * | 协作类型:0-跟随data_scope,1-只读,2-读写 |
| expire_at | object |  | 过期时间 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### UserResourceCollaborationUpdate

更新用户资源协作请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| status | object |  | 状态:1-Active,2-InActive |
| access_type | object |  | 协作类型:0-跟随data_scope,1-只读,2-读写 |
| expire_at | object |  | 过期时间 |

### UserResourceCollaborationWithDetailResponse

用户资源协作响应（包含详细信息） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 协作ID |
| granter_id | integer | * | 授权人ID |
| granter_name | string |  | 授权人用户名 |
| granter_real_name | string |  | 授权人真实姓名 |
| grantee_id | integer | * | 被授权人ID |
| grantee_name | string |  | 被授权人用户名 |
| grantee_real_name | string |  | 被授权人真实姓名 |
| status | integer | * | 状态:1-Active,2-InActive |
| access_type | integer | * | 协作类型:0-跟随data_scope,1-只读,2-读写 |
| expire_at | object |  | 过期时间 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### UserResourceScopeBatchCreate

批量创建用户资源范围关系请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| resource_scope_ids | array<integer> | * | 资源范围ID列表 |

### UserResourceScopeCreate

创建用户资源范围关系请求 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| user_id | integer | * | 用户ID |
| resource_scope_id | integer | * | 资源范围ID |

### UserResourceScopeResponse

用户资源范围关系响应 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 关系ID |
| user_id | integer | * | 用户ID |
| resource_scope_id | integer | * | 资源范围ID |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### UserResourceScopeWithDetailResponse

用户资源范围关系响应（包含详细信息） 

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| id | integer | * | 关系ID |
| user_id | integer | * | 用户ID |
| user_name | string |  | 用户名 |
| user_real_name | string |  | 用户真实姓名 |
| resource_scope_id | integer | * | 资源范围ID |
| resource_scope_name | string |  | 资源范围名称 |
| resource_scope_status | integer |  | 资源范围状态 |
| resource_type_id | integer |  | 资源类型ID |
| resource_type_name | string |  | 资源类型名称 |
| created_at | date-time(string) | * | 创建时间 |
| updated_at | date-time(string) | * | 更新时间 |

### UserResponse

用户响应

### UserStatusUpdate

更新用户状态请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| status | integer | * | 状态:1-Active,2-InActive |

### UserUpdate

更新用户请求

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| real_name | object |  | 真实姓名 |
| email | object |  | 邮箱 |
| mobile | object |  | 联系电话 |
| status | object |  | 状态:1-Active,2-InActive |
| is_superuser | object |  | 是否超级用户:0-普通,1-超级 |
| department_ids | object |  | 部门ID列表 |
| position_ids | object |  | 职务ID列表 |

### ValidationError

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| loc | array<object> | * |  |
| msg | string | * |  |
| type | string | * |  |

