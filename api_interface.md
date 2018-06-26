# API for IFusion

**This is The API document for IFusion project.**

--------------------------------------------------------

##Table of Contents

[toc]

--------------------------------------------------------
## 1. Workspace

有关Workspace（工作区）的操作

### 1.1 展示登录用户的所有工作区

```
GET /workspace/all

Accept:         application/json

Response:
  {
    "code":0,
    "workspaces": [ // 工作区列表，按创建时间倒序排序
      {
        "workspaceId": "<string> 工作区ID",
        "title": "<string> 工作区标题",
        "createdBy": "<string> 创建用户ID",
        "users": [ // 对该工作区拥有权限的用户列表
          {
            "userId": "<string> 用户ID",
            "access": "<string> 用户权限"
          }
        ],
        "createdTime": "<long> 工作区创建时间",
        "createdTimeFamat": "<yyyy-MM-dd HH:mm> 格式化后的创建时间",
      }
    ]
  }
```
<br/>

### 1.2 根据ID查询特定工作空间

```
GET /workspace

Accept:         application/json

Params:
  workspaceId   工作区ID，必需

Response:
  {
    "code":0,
    "workspaceId": "<string> 工作区ID",
    "title": "<string> 工作区标题",
    "createdBy": "<string> 创建用户ID",
    "users": [ // 对该工作区有权限的用户信息列表
      {
        "userId": "<string> 创建用户ID",
        "access": "<string> 用户权限"
      }
    ],
    "createdTime": "<long> 创建时间",
    "createdTimeFamat": "<yyyy-MM-dd HH:mm> 格式化后的创建时间",
  }
```
<br/>

### 1.3 创建或更新工作空间

```
POST /workspace

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  workspaceId   工作区ID，必需
  displayTitle  工作区标题，必需

Response:
  {
    "code":0,
    "workspaceId": "<string> 工作区ID",
    "title": "<string> 工作区标题",
    "createdBy": "<string> 创建用户ID",
    "users": [ // 对该工作区有权限的用户信息列表
      {
        "userId": "<string> 创建用户ID",
        "access": "<string> 用户权限"
      }
    ],
    "createdTime": "<long> 创建时间",
    "createdTimeFamat": "<yyyy-MM-dd HH:mm> 格式化后的创建时间",
  }
```
<br/>

### 1.4 更改工作空间的用户权限

```
POST /workspace/chown

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  workspaceId       工作区ID，必需
  userId            工作区标题，必需
  workspaceAccess   用户权限，int，必需
                      权限说明：
                      删除权限      0
                      读取权限      1
                      评论权限      2
                      编辑权限      3

Response:
  {
    "code":0,
    "workspaceId": "<string> 工作区ID",
    "title": "<string> 工作区标题",
    "createdBy": "<string> 创建用户ID",
    "users": [ // 对该工作区有权限的用户信息列表
      {
        "userId": "<string> 创建用户ID",
        "access": "<string> 用户权限"
      }
    ],
    "createdTime": "<long> 创建时间",
    "createdTimeFamat": "<yyyy-MM-dd HH:mm> 格式化后的创建时间",
  }
```
<br/>

### 1.5 删除工作区

```
DELETE /workspace

Accept:         application/json

Params:
  workspaceId       工作区ID，必需

Response:
  {
    "code":0
  }
```

--------------------------------------------------------

## 2. Graph

有关Graph的操作

### 2.1 查询某工作区下所有Graph基本信息

```
GET /graph/all

Accept:         application/json

Params:
  workspaceId       工作区ID，必需

Response:
  {
    "code":0
    "products": [ // 该工作区下所有Graph的信息列表，按创建时间倒序排序
      {
        "id": "<string> GraphCase ID",
        "title": "<string> Graph名称",
        "workspaceId": "<string> Graph所属工作区ID",
        "createdTime": "<long> 创建时间",
        "createdTimeFormat": "<yyyy-MM-dd HH:mm> 格式化后创建时间"
      }
    ]
  }
```
<br/>

### 2.2 查询某工作区下指定Graph的详细信息

```
GET /graph

Accept:         application/json

Params:
  graphId       graph ID，必需

Response:
  {
    "code":0,
    "id": "<string> Graph ID",
    "workspaceId": "<string> Graph所属工作区ID",
    "title": "<string> Graph名称",
    "extendedData": { // 产品详细信息（其中的点、边等信息）
      "vertices": [ // 产品中顶点信息列表
        {
          "pos": { // 点的位置
            "x": "<double> x坐标",
            "y": "<double> y坐标"
          },
          "id": "<string> 顶点ID"
        },
      ],
      "edges": [ // 产品中边的信息列表
        {
          "edgeId": "<string> 边ID",
          "inVertexId": "<string> 入点ID",
          "label": "<string> 边类型(OWL格式)",
          "outVertexId": "<string> 出点ID"
        }
      ]
    },
    "createdTime": "<long> 创建时间",
    "createdTimeFormat": "<yyyy-MM-dd HH:mm> 格式化后创建时间"
  }
```
<br/>

### 2.3 创建或更新Graph

```
POST /graph

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  graphId         graph ID，可选
  workspaceId     该Graph所属的工作区ID，必需
  title           该Graph名称，必需

Response:
  {
    "code":0,
    "id": "<string> GraphCase ID",
    "title": "<string> Graph名称",
    "workspaceId": "<string> Graph所属工作区ID",
    "createdTime": "<long> 创建时间",
    "createdTimeFormat": "<yyyy-MM-dd HH:mm> 格式化后创建时间"
  }
```
<br/>

### 2.4 新建或移动Graph上的点

```
POST /graph/vertexUpdate

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  graphId       graph ID，必需
  vertexId      要更新的顶点ID，必需
  x             更新后点的x坐标，double，必需
  y             更新后点的y坐标，double，必需

Response:
  {
    "code":0
  }
```
<br/>

### 2.5 移除Graph上的点

```
POST /graph/vertexRemove

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  graphId       graph ID，必需
  vertexId      要移除的顶点ID，必需

Response:
  {
    "code":0
  }
```
<br/>

### 2.6 删除Graph

```
DELETE /graph

Accept:         application/json

Body:
  graphId       要删除的graph ID，必需

Response:
  {
    "code":0
  }
```

------------------------------------

## 3 Vertex

有关顶点的操作

### 3.1 查询与某顶点相关的关系(边)及对应顶点

```
POST /vertex/edges

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  vertexId      要查询的顶点ID，必需
  edgeLabel     要查询的关系类型(OWL格式schema)，可选
  limit         结果集限制数量，必需
  offset        结果集起始偏移量，必需

Response:
  {
    "code":0,
    "totalHits":"<int> 总共查到的关系数目",
    "relationships": [ // 关系详细信息列表
      {
        "relationship": { // 关系详细信息
          "id": "<string> 关系ID",
          "properties": [ // 该边属性详细信息列表
            {
              "key": "<string> 属性ID",
              "schema": "<string> 属性schema（OWL本体）",
              "value": "属性值，不同属性不同类型"
            }
          ],
          "visibility": "<string> 可见性",
          "conceptType": "<string> 关系类型（OWL格式）",
          "outVertexId": "<string> 出点ID",
          "inVertexId": "<string> 入点ID"
        },
        "vertex": { // 对应顶点详细信息
          "id": "<string> 顶点ID",
          "properties": [ // 该顶点属性详细信息列表
            {
              "key": "<string> 属性ID",
              "schema": "<string> 属性schema（OWL本体）",
              "value": "属性值，不同属性不同类型"
            }
          ],
          "visibility": "<string> 可见性",
          "conceptType": "<string> 顶点类型（OWL本体）"
        }
      }
    ]
  }
```
<br/>

### 3.2 查询一个或一些顶点信息

```
POST /vertex/multiple

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  vertexIds     要查询的顶点ID，JSON格式字符串数组，必需

Response:
  {
    "code":0,
    "vertices": [ // 顶点详细信息列表
      {
        "conceptType": "<string> 顶点类型（OWL本体）",
        "id": "<string> 顶点ID",
        "properties": [// 顶点属性详细列表
          {
            "key": "<string> 属性ID",
            "schema": "<string> 属性schema（OWL本体）",
            "value": "属性值，不同属性不同类型"
          }
        ],
        "visibility": "<string> 可见性"
      }
    ]
  }
```
<br/>

### 3.3 新增顶点

```
POST /vertex/create

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  graphId             新增顶点所属图ID，必需
  conceptType         新增顶点的类型，必需
  vertexId            新增顶点ID，可选
  visibility          新增顶点的可见性，必需
  justificationText   新增顶点理由，必需

Response:
  {
    "code":0,
    "conceptType": "<string> 顶点类型（OWL本体）",
    "id": "<string> 顶点ID",
    "properties": [// 顶点属性详细列表
      {
        "key": "<string> 属性ID",
        "schema": "<string> 属性schema（OWL本体）",
        "value": "属性值，不同属性不同类型"
      }
    ],
    "visibility": "<string> 可见性",
  }
```
<br/>

### 3.4 增加或修改顶点属性

```
POST /vertex/property

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  vertexId          要修改的顶点ID，必需
  propertyID        要修改的属性ID，可选
  propertySchema    要新增/修改的属性schema，必需
  value             要新增/修改的属性值，必需

Response:
  {
    "code":0,
    "conceptType": "<string> 顶点类型（OWL本体）",
    "id": "<string> 顶点ID",
    "properties": [// 顶点属性详细列表
      {
        "key": "<string> 属性ID",
        "schema": "<string> 属性schema（OWL本体）",
        "value": "属性值，不同属性不同类型"
      }
    ],
    "visibility": "<string> 可见性",
  }
```
<br/>

### 3.5 删除某个顶点的某个特定属性

```
DELETE /vertex/property

Accept:         application/json

Params:
  vertexId          要修改的顶点ID，必需
  propertyID        要删除的属性ID，必需
  propertySchema    要删除的属性schema，必需

Response:
  {
    "code":0
  }
```
<br/>

### 3.6 删除顶点

```
DELETE /vertex

Accept:         application/json

Params:
  vertexId      要删除的顶点ID，必需

Response:
  {
    "code":0
  }
```
<br/>

### 3.7 查询当前所有实体统计信息

```
GET /vertex/statistics

Accept:         application/json

Response:
  {
    "code":0,
    "statistics":[ // 具体的各类实体统计信息列表
      {
        "conceptType": "<string> 顶点类型（OWL本体）",
        "count":"<int> 该类型顶点数量"
      }
    ]
  }
```

------------------------------------

## 4. Edge

有关关系（边）的操作

### 4.1 查询一个或一些关系（边）信息

```
POST /edge/multiple

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  edgeIds          要查询的边ID，JSON格式字符串数组，必需

Response:
  {
    "code":0,
    "edges": [ // 边详细信息列表
      {
        "id": "<string> 边ID",
        "properties": [ // 边属性详细信息列表
          {
            "key": "<string> 属性ID",
            "schema": "<string> 属性schema（OWL本体）",
            "value": "属性值，不同属性不同类型"
          }
        ],
        "visibility": "<string> 可见性",
        "edgeLabel": "<string> 该边的类型（OWL本体）",
        "outVertexId": "<string> 出点ID",
        "inVertexId": "<string> 入点ID"
      }
    ]
  }
```
<br/>

### 4.2 新增关系(边)

```
POST /edge/create

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  conceptType           新增边的类型，必需
  outVertexId           出点ID，必需
  inVertexId            入点ID，必需
  visibility            新增边的可见性，必需
  justificationText     新增边理由，必需

Response:
  {
    "code":0,
    "id": "<string> 边ID",
    "conceptType": "<string> 该边的类型（OWL本体）",
    "properties": [ // 边属性详细信息列表
      {
        "key": "<string> 属性ID",
        "schema": "<string> 属性schema（OWL本体）",
        "value": "属性值，不同属性不同类型"
      }
    ],
    "visibility": "<string> 可见性",
    "outVertexId": "<string> 出点ID",
    "inVertexId": "<string> 入点ID"
  }
```
<br/>

### 4.3 增加或更新关系(边)属性

```
POST /edge/property

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  edgeId            要修改的边ID，必需
  propertyID        要修改的属性ID，可选
  propertySchema    要新增/修改的属性schema，必需
  value             要新增/修改的属性值，必需

Response:
  {
    "code":0,
    "id": "<string> 边ID",
    "conceptType": "<string> 该边的类型（OWL本体）",
    "properties": [ // 边属性详细信息列表
      {
        "key": "<string> 属性ID",
        "schema": "<string> 属性schema（OWL本体）",
        "value": "属性值，不同属性不同类型"
      }
    ],
    "visibility": "<string> 可见性",
    "outVertexId": "<string> 出点ID",
    "inVertexId": "<string> 入点ID"
  }
```
<br/>

### 4.4 删除某个关系(边)的某个特定属性

```
DELETE /edge/property

Accept:         application/json

Body:
  edgeId            要修改的边ID，必需
  propertyID        要删除的属性ID，可选
  propertySchema    要删除的属性schema，必需

Response:
  {
    "code":0
  }
```
<br/>

### 4.5 删除边

```
DELETE /edge/property

Accept:         application/json

Body:
  edgeId        要删除的边ID，必需

Response:
  {
    "code":0
  }
```
<br/>

### 4.6 查询当前所有关系的统计信息

```
GET /edge/statistics

Accept:         application/json

Response:
  {
    "code":0,
    "statistics":[ // 具体的各类关系统计信息列表
      {
        "conceptType": "<string> 关系类型（OWL本体）",
        "count":"<int> 该类型的关系数量"
      }
    ]
  }
```

------------------------------------------------

## 5. 耗时任务

以下为一些复杂的耗时任务

### 5.1 查找两顶点间路径

```
POST /findPath

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  outVertexId     出点ID，必需
  inVertexId      入点ID，必需
  maxHops         最大跳数，必需
  edgeLabels      要查询的边的类型，JSON格式字符串数组，必需

Response:
  {
    "code":0,
    "taskID":"<string> 任务ID",
    "type":"任务类型"
  }
```
<br/>

### 5.2 根据指定条件建立查询顶点的任务

```
POST /vertex/search

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  q               查询关键字，必需
  conceptTypes    要查询的顶点类型，可选
  order           排序方式，可选
  filter          过滤条件，可选

order具体形式：JSON格式字符串
{
  "<string> OWL格式属性schema":"DESCENDING / ASCENDING"
}
filter具体形式：JSON格式字符串
{
  "hasProp":["<string> OWL格式的属性schema列表"], // 包含某些属性，可以为空
  "hasNotProp":["<string> OWL格式的属性schema列表"], // 不包含某些属性，可以为空
  "contains":[ // 某属性包含某内容列表
    {
      "<string> OWL格式的属性schema":"<string> 要过滤的属性内容"
    }
  ]
}

Response:
  {
    "code":0,
    "taskID":"<string> 任务ID",
    "type":"任务类型"
  }
```
<br/>

### 5.3 根据指定条件建立查询边的任务

```
POST /edge/search

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
  q               查询关键字，必需
  conceptTypes    要查询的关系类型，可选
  order           排序方式，可选
  filter          过滤条件，可选

order具体形式：JSON格式字符串
{
  "<string> OWL格式属性schema":"DESCENDING / ASCENDING"
}
filter具体形式：JSON格式字符串
{
  "hasProp":["<string> OWL格式的属性schema列表"], // 包含某些属性，可以为空
  "hasNotProp":["<string> OWL格式的属性schema列表"], // 不包含某些属性，可以为空
  "contains":[ // 某属性包含某内容列表
    {
      "<string> OWL格式的属性schema":"<string> 要过滤的属性内容"
    }
  ]
}

Response:
  {
    "code":0,
    "taskID":"<string> 任务ID",
    "type":"任务类型"
  }
```
<br/>

### 5.4 获取顶点查询任务统计结果

```
GET /vertex/count

Accept:         application/json

Params:
  taskID        任务ID，必需
  type          "vertexCount"，任务类型，必需

Response:
  {
    "code":0,
    "taskStatus":"<string> 任务处理状态",
    "totalHits":"<int> 总共查到的顶点数目"
  }
```
<br/>

### 5.5 获取顶点查询任务详细结果

```
GET /vertex/result

Accept:         application/json

Params:
  taskID      任务ID，必需
  type        "vertexResult"，任务类型，必需
  limit       结果集限制数量，必需
  offset      结果集起始偏移量，必需

Response:
  {
    "code":0,
    "taskStatus":"<string> 任务处理状态",
    "nextOffset": "<int> 下一个偏移量",
    "vertices": [ // 查到的顶点详细信息列表
      {
        "conceptType": "<string> 顶点schema（OWL本体）",
        "id": "<string> 顶点ID",
        "properties": [ // 顶点属性详细列表
          {
            "key": "<string> 属性ID",
            "schema": "<string> 属性schema（OWL本体）",
            "value": "属性值，不同属性不同类型"
          }
        ],
        "visibility": "<string> 可见性",
      }
    ]
  }
```
<br/>

### 5.6 获取关系（边）查询任务统计结果

```
GET /edge/count

Accept:         application/json

Params:
  taskID      任务ID，必需
  type        "edgeCount"，任务类型，必需

Response:
  {
    "code":0,
    "taskStatus":"<string> 任务处理状态",
    "totalHits":"<int> 总共查到的关系数目"
  }
```
<br/>

### 5.7 获取关系（边）查询任务详细结果

```
GET /vertex/result

Accept:         application/json

Params:
  taskID      任务ID，必需
  type        "edgeResult"，任务类型，必需
  limit       结果集限制数量，必需
  offset      结果集起始偏移量，必需

Response:
  {
    "code":0,
    "taskStatus":"<string> 任务处理状态",
    "nextOffset": "<int> 下一个偏移量",
    "edges": [ // 查到的关系详细信息列表
      {
        "conceptType": "<string> 关系schema（OWL本体）",
        "id": "<string> 关系ID",
        "properties": [ // 关系属性详细列表
          {
            "key": "<string> 属性ID",
            "schema": "<string> 属性schema（OWL本体）",
            "value": "属性值，不同属性不同类型"
          }
        ],
        "visibility": "<string> 可见性",
      }
    ]
  }
```
<br/>

-----------------------------------------------------
## 6. user

user相关的操作

### 6.1 获取所有用户列表

```
GET /user/all

Accept:application/json

Response:
{
    "code": 0,
    "users": [ // 可分享的用户信息列表
        {
            "id": "<string> 用户ID",
            "userName": "<string> 用户名",
            "displayName": "<string> 前端展示用户名",
            "userType": "<string> 用户类型",
            "currentWorkspaceId": "<string> 当前工作区ID",
            "currentWorkspaceName": "<string> 当前工作区名称",
            "privileges": ["<string> 该用户拥有权限列表"],
            "workspaces": ["<string> 该用户可见的workspace ID列表"],
        }
    ]
}
```
<br/>

### 6.2 获取当前登录用户的信息

```
GET /user/me

Accept:application/json

Response:
{
    "code":0,
    "id": "<string> 用户ID",
    "userName": "<string> 用户名",
    "displayName": "<string> 前端展示用户名",
    "userType": "<string> 用户类型",
    "currentWorkspaceId": "<string> 当前工作区ID",
    "currentWorkspaceName": "<string> 当前工作区名称",
    "privileges": ["<string> 该用户拥有权限列表"],
    "workspaces": ["<string> 该用户可见的workspace ID列表"],
}
```
<br/>

### 6.3 设置用户界面默认参数

```
POST /user/ui-preferences

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body:
    userId:用户id   string，必需
    value:属性值    json串，如{"key":"value"}，必需

Response:
    {
        "code":0
    }
```
<br/>

### 6.4 添加用户

```
POST /user/add

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    username:用户名       string，必需
    password:用户密码     string，必需
    privileges:用户权限   string（","隔开），必需

Response:
    {
        "code": 0,
        "privileges": ["<string> 该用户拥有权限列表"],
        "displayName": "test4", //显示的用户名
        "userName": "test4", //用户名
        "userId": "USER_f3b143fddff146b3a1ed7e09aaefe32e", //用户id
        "createDate": 1529038192688 //创建时间
    }
```
<br/>


### 6.5 在用户管理中查询满足搜索条件的用户

```
POST /user/search

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    q:需要查询的用户名（模糊）  string，必需
    startDate:需要搜索用户的创建时间的起始时间    string，可选
    endDate:需要搜索用户的创建时间的截止时间    string，可选
    auth:需要搜索包含该权限的用户 string，可选
    offset:查询结果偏移量 int，可选，默认值0
    size:返回结果集大小   int，可选，默认值7

Response:
    {
        "code": 0,
        "elements": [  //用户的详细信息
            {
                "privileges": ["<string> 该用户拥有权限列表"],
                "id": "USER_c572422bc6e04a2cafc4968469d020d9",//用户id
                "userName": "test2",//用户名
                "createDate": 1528874091208 //该用户创建时间
            }
        ],
        "nextOffset": 7, //下一次查询结果的偏移
        "itemCount": 2  //返回的用户数目
    }
```
<br/>


### 6.6 修改用户权限

```
POST /user/update/privileges

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    userId： 要修改权限的用户id  string，必需
    privileges：用户权限  string（","隔开），必需

Response:
    {
        "code":0,
        "privileges": [ //用户权限
            "ADMIN",
            "COMMENT",
            "EDIT",
            "HISTORY_READ",
            "ONTOLOGY_PUBLISH",
            "PUBLISH",
            "READ",
            "SEARCH_SAVE_GLOBAL"
        ]
    }
```
<br/>


### 6.7 修改用户密码

```
POST /user/update/password

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    userId：用户id）  string，必需
    type: 用户类型    string，必需
    currentpassword : 当前密码    string，可选(管理员不需要)
    newpassword: 新密码           string，必需

Response:
    {
        "code": 0
    }
```
<br/>

### 6.8 删除用户

```
DELETE /user/delete

Accept:         application/json

Body：
    userId：需要删除的用户id（之前使用username）  string，必需

Response:
    {
        "code": 0
    }
```
<br/>

-----------------------------------------------------

## 7. ontology

ontology相关的操作

### 7.1 读取本体(Ontology)信息

```
GET /ontology/all

Accept:application/json

Response:
{
    "code": 0,
    "concepts": [ // 实体本体信息列表
        {
            "id": "<string> 实体本体ID（OWL格式）",
            "title": "<string> 实体本体标题（OWL格式）",
            "displayName": "<string> 实体本体展示名称",
            "displayType": "<string> 实体本体展示类型",
            "titleFormula": "prop('http://www.isinonet.com/demo#name') || ''", 
            "parentConcept": "<string> 父本体（OWL格式）",
            "pluralDisplayName": "<string> 复数展示名称",
            "userVisible": "<boolean> 是否用户可见",
            "glyphIconHref": "<string> 图标链接",
            "color": "<string> 颜色（RGB或16进制数）",
            "deleteable": "<boolean> 是否可删除",
            "updateable": "<boolean> 是否可更新",
            "properties": [ "<string> 拥有的属性名列表（OWL格式）" ],
            "metadata": { // 元数据
                "<string> 属性名（OWL格式）": "<string> 属性值"
            }
        }
    ],
    "properties": [ // 属性本体信息列表
        {
            "title": "<string> 属性本体标题（OWL格式）",
            "displayName": "<string> 展示名称",
            "userVisible": "<boolean> 是否用户可见",
            "searchable": "<boolean> 是否可搜索",
            "addable": "<boolean> 是否可添加",
            "sortable": "<boolean> 是否可排序",
            "dataType": "<string> 数据类型",
            "deleteable": "<boolean> 是否可删除",
            "updateable": "<boolean> 是否可更新",
            "textIndexHints": [ 
                "EXACT_MATCH",
                "FULL_TEXT"
            ]
        }
    ],
    "relationships": [ // 关系本体信息列表
        {
            "parentIri": "http://www.w3.org/2002/07/owl#topObjectProperty", 
            "title": "<string> 关系本体标题（OWL格式）",
            "displayName": "<string> 关系本体展示名称",
            "userVisible": "<boolean> 是否用户可见",
            "updateable": "<boolean> 是否可更新",
            "deleteable": "<boolean> 是否可删除",
            "domainConceptIris": [ 
                "http://www.isinonet.com/insider#actor"
            ],
            "rangeConceptIris": [ 
                "http://www.isinonet.com/insider#event"
            ],
            "properties": [
                "<string> 该关系所具有的所有属性的OWL定义"
            ]
        }
    ]
}
```
<br/>

### 7.2 新建owl实体

```
POST /ontology/create/concept

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    displayName:新建的实体名称    string，必需
    parentConcept:继承的父类  string，必需
    glphlconHref:图标 string，必需
    color:图标颜色RGB string，必需

Response:
    {
        "code": 0,
        "id": "<string> 实体本体ID（OWL格式）",
        "title": "<string> 实体本体标题（OWL格式）",
        "displayName": "<string> 实体本体展示名称",
        "displayType": "<string> 实体本体展示类型",
        "titleFormula": "prop('http://www.isinonet.com/demo#name') || ''",
        "parentConcept": "<string> 父本体（OWL格式）",
        "pluralDisplayName": "<string> 复数展示名称",
        "userVisible": "<boolean> 是否用户可见",
        "glyphIconHref": "<string> 图标链接",
        "color": "<string> 颜色（RGB或16进制数）",
        "deleteable": "<boolean> 是否可删除",
        "updateable": "<boolean> 是否可更新",
        "properties": [ "<string> 拥有的属性名列表（OWL格式）" ],
        "metadata": { // 元数据
            "<string> 属性名（OWL格式）": "<string> 属性值"
        }
    }
```
<br/>

### 7.3 新建owl关系

```
POST /ontology/create/relationship

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    displayName:本体关系名称，必需
    sourceIris:出度实体，必需
    targetIris:入度实体，必需

Response:
    {
        "code": 0,
        "parentIri": "http://www.w3.org/2002/07/owl#topObjectProperty", 
        "title": "<string> 关系本体标题（OWL格式）",
        "displayName": "<string> 关系本体展示名称",
        "userVisible": "<boolean> 是否用户可见",
        "updateable": "<boolean> 是否可更新",
        "deleteable": "<boolean> 是否可删除",
        "domainConceptIris": [ 
            "http://www.isinonet.com/insider#actor"
        ],
        "rangeConceptIris": [
            "http://www.isinonet.com/insider#event"
        ],
        "properties": [
            "<string> 该关系所具有的所有属性的OWL定义"
        ]
    }
```
<br/>

### 7.4 新建owl属性

```
POST /ontology/create/property

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    displayName:属性的显示名称    string，必需
    dataType:属性类型     string，必需
    relationshipIris:所属本体关系的标志     string，可选（至少conceptIris二选一）
    conceptIris:所属本体实体的标志  string，可选（至少relationshipIris二选一）
注意：relationshipIris和conceptIris存在多个用","分隔

Response:
    {
        "code": 0,
        "title": "<string> 属性本体标题（OWL格式）",
        "displayName": "<string> 展示名称",
        "userVisible": "<boolean> 是否用户可见",
        "searchable": "<boolean> 是否可搜索",
        "addable": "<boolean> 是否可添加",
        "sortable": "<boolean> 是否可排序",
        "dataType": "<string> 数据类型",
        "deleteable": "<boolean> 是否可删除",
        "updateable": "<boolean> 是否可更新",
        "textIndexHints": [
            "EXACT_MATCH",
            "FULL_TEXT"
        ]
    }
```
<br/>

### 7.5 根据本体实体id查找其详细信息。

```
GET /ontology/search/concept

Accept:application/json

Params:
    conceptIds:查询的owl关系的id    string 多个id用","隔开，必需

Response:
    {
        "code": 0,
        "concepts": [  //本体详细信息列表
            {
                "id": "http://www.isinonet.com/concept1#09579e9107fa42a29fe0a520063f01b0", //实体id
                "title": "http://www.isinonet.com/concept1#09579e9107fa42a29fe0a520063f01b0", //实体title
                "displayName": "concept新建1", //新建的实体显示名称
                "parentConcept": "http://www.isinonet.com/#f74e68e01f67407c9613b1ee8acfae9b", //继承的父类
                "pluralDisplayName": "concept新建1s", //
                "glyphIconHref": "resource?id=http%3A%2F%2Fwww.isinonet.com%2Fconcept1%2309579e9107fa42a29fe0a520063f01b0",//
                "color": "#409EEF", //颜色信息
                "deleteable": true, //可删除
                "updateable": true, //可更新
                "properties": [], //属性信息
                "metadata": {
                    "http://www.isinonet.com#modifiedBy": "USER_290d080426b24782a1efe48c9f6ab709", //修改的用户
                    "http://www.isinonet.com#glyphIconFileName": "img/glyphicons/glyphicons-441-folder-closed@2x.png", //图信息
                    "http://www.isinonet.com#visibilityJson": "{\"source\":\"(ontology)|visallo\"}", //可见性
                    "http://www.isinonet.com#modifiedDate": "Fri Jun 15 05:38:39 UTC 2018" //修改时间
                }
            }
        ],
    }
```
<br/>

### 7.6 根据本体关系id查找其详细信息。

```
GET /ontology/search/relationship

Accept:application/json

Params:
    relationshipIds:查询的owl关系的id    string 多个id用","隔开，必需

Response:
    {
        "code": 0,
        "relationships": [  //
            {
                "parentIri": "http://www.w3.org/2002/07/owl#topObjectProperty", // TODO OWL定义根文件？
                "title": "<string> 关系本体标题（OWL格式）",
                "displayName": "<string> 关系本体展示名称",
                "userVisible": "<boolean> 是否用户可见",
                "updateable": "<boolean> 是否可更新",
                "deleteable": "<boolean> 是否可删除",
                "domainConceptIris": [ // TODO
                    "http://www.isinonet.com/insider#actor"
                ],
                "rangeConceptIris": [ // TODO
                    "http://www.isinonet.com/insider#event"
                ],
                "properties": [
                    "<string> 该关系所具有的所有属性的OWL定义"
                ]
            }，
        ]
    }
```
<br/>

### 7.7 根据本体属性id返回查找其详细信息。

```
GET /ontology/search/property

Accept:application/json

Params:
    propertyIds:查询的属性的id    string 多个id用","隔开，必需

Response:
    {
        "code": 0,
        "concepts": [
            {
                "title": "<string> 属性本体标题（OWL格式）",
                "displayName": "<string> 展示名称",
                "userVisible": "<boolean> 是否用户可见",
                "searchable": "<boolean> 是否可搜索",
                "addable": "<boolean> 是否可添加",
                "sortable": "<boolean> 是否可排序",
                "dataType": "<string> 数据类型",
                "deleteable": "<boolean> 是否可删除",
                "updateable": "<boolean> 是否可更新",
                "textIndexHints": [
                    "EXACT_MATCH",
                    "FULL_TEXT"
                ]
            },
        ]
    }
```
<br/>

-----------------------------------------------------

## 8. 文件导入查询的相关操作

### 8.1 查询导入的文件

```
POST /file/search

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    q:要搜索文件名    string,必需
    startDate:查询时间范围的起始时间  string，可选
    endDate:查询时间范围的截止时间    string，可选
    size:返回结果条数 int，可选 默认7
    offset:结果偏移量   int，可选 默认0
    fileSize:要搜索的文件大小 string，可选

Response:
    {
        "code": 0,
        "elements": [  //查询到的文件的信息
            {
                "creator": "admin", //该文件的上传者
                "fileSize": "28.4MB", //该文件的大小
                "uploadedTime": 1527481194340, //该文件上传的时间
                "name": "gdelt-523- 1610.np", //该文件的名称
                "id": "f296ef77ba0146a0871e64508d995f74"  //该上传文件的id
            }
        ],
        "nextOffset": 7,  //下次查询的offset
        "itemCount": 1  //查询到的文件数量
    }
```
<br/>

-----------------------------------------------------

## 9. visibility

可见性管理相关操作

### 9.1 新建或者修改可见性名称

```
POST /visibility/update

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    username：需要添加的用户  string，必需
    auth：可见性名称  string，必需

Response:
    {
        "code":0,
        "creator": "admin", //创建者
        "name": "test3", //可见性名称
        "count": 2, //授权用户数
        "createdTime": 1529033527016, //创建时间
        "id": "e584e405ea144db98d59fa4953b0a032", //该可见性id
        "users": "admin,sys" //授权的用户
    }
```
<br/>

### 9.2 查询可见性名称

```
POST /visibility/search

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    q: 搜索可见性名称（可模糊查询）   string，必需
    startDate: 要查询的创建的起始时间 string，可选
    endDate: 要查询的创建的截止时间   string，可选
    count: 授权用户数量   string，可选
    offset: 查询的结果集偏移  int，可选  默认0
    size: 返回查询结果数  int，可选  默认7

Response:
    {
        "code": 0,
        "elements": [ // 查到的可见性详细信息列表
            {
                "creator": "admin", //创建可见性者
                "name": "测试", //可见性名称
                "count": 2, //可见性用户个数
                "createdTime": 1528873779850, //创建该可见性的时间
                "id": "80c27dfad9134090904d568d28ad00f9", //可见性id
                "users": "sys,root" //可见性用户
            },
        ],
        "nextOffset": 7, //下一页起始偏移量
        "itemCount": 2 //返回的元素个数
    }
```
<br/>

### 9.3 删除可见性用户

```
DELETE /visibility/remove

Accept:         application/json

Body：
    auth:可见性名称 string，必需

Response:
    {
        "code": 0
    }
```
<br/>

--------------------------------------------------------

## 10. Dashboard

Dashboard相关的操作

### 10.1 根据条件获取仪表盘中的通知内容

```
POST /notification/all

Content-Type:   application/x-www-form-urlencoded
Accept:         application/json

Body：
    offset:查询结果的偏移量 int，默认 0
    limit：查询保留结果条数 int，默认20

Response:
    {
        "code": 0,
        [
            {
                "messageType":<string>,必需 （"system"或者”user“)
                "sentDate": "Mon May 28 04:14:13 UTC 2018", //通知日期
                "id": "1527480853580:f0ec58cc-8453-46fb-9810-0bf8707581e8", //通知事件id
                "title": "操作权限变更",                                //操作的主题
                "message": "新的操作权限列表: 编辑, 评论, 发布, 可见性",    //操作的内容信息
                "userId": "USER_290d080426b24782a1efe48c9f6ab709",    //用户的id
            },
            {
                "messageType":<string>,必需 （"system"或者”user“)
                "sentDate": "Mon May 28 04:14:13 UTC 2018", //通知日期
                "id": "1527480853580:f0ec58cc-8453-46fb-9810-0bf8707581e8", //通知事件id
                "title": "操作权限变更",                                //操作的主题
                "message": "新的操作权限列表: 编辑, 评论, 发布, 可见性",    //操作的内容信息
                "userId": "USER_290d080426b24782a1efe48c9f6ab709",    //用户的id
            },
        ]
    }
```
<br/>

