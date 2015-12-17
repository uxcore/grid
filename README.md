# uxcore-table

---

Table UI Component based on React. working for many modes such as sub table, tree table and inline editing table.


![](demo/screenshot.png)

## How to run

```sh
$ git clone https://github.com/uxcore/uxcore-table
$ cd uxcore-table
$ npm install
$ gulp server
```

## Best Practice

```javascript

	let columns = [
        { dataKey: 'check', type: 'checkbox', disable: false}, // custom checkbox column, dataKey can be anyone, true means checked.
	    { dataKey: 'id', title: 'ID', width: 50,hidden:true},
	    { dataKey: 'country', title:'国家', width: 200,ordered:true},
	    { dataKey: 'city',title:'城市', width: 150,ordered:true },
	    { dataKey: 'firstName',title:"FristName" },  
	    { dataKey: 'lastName' ,title:"LastName"},
	    { dataKey: 'email',title:"Email",width: 200,ordered:true }
	];


	let rowSelection = {
      onSelect: function(record, selected, selectedRows) {
        console.log(record, selected, selectedRows);
      },
      onSelectAll: function(record, data) {
        console.log(record, data);
      }
    };

	renderProps={
        actionBar: {
           'new': function(type, actions){ alert(type); },  // type means 'new' in this line
           'import': function(type, actions){ alert(type); }, // actions contains all table's APIs, such as actions.addEmptyRow()
           'export': function(type, actions){ alert(type); }
        },
        fetchUrl:"http://localhost:3000/demo/data.json",
        jsxcolumns:columns,
        subComp:(<Table {...renderSubProps}  ref="subTable"/>),
        rowSelection: rowSelection
	},
	
	renderSubProps={
        jsxcolumns:columns,
        fetchUrl:"http://localhost:3000/demo/data.json",
        queryKeys:["dataKey","firstName"],
        onModifyRow: this.onModifyRow
	};

	<Table {...renderProps} />

```



## Props

|Name            |Type                |Require   |Default|Note |
|---             |---                 |---       |---    |---|
|jsxcolumns      |array               |required  |null   |表格列配置项，具体见[这里]()|
|width           |number              |optional  |1000   |表格的宽度|
|height          |number              |optional  |100%   |表格的高度|
|showColumnPicker|boolean             |optional  |true   |是否显示列筛选按钮|
|showPager       |boolean             |optional  |true   |是否显示分页|
|showHeader      |boolean             |optional  |true   |是否显示表格头部|
|showMask        |boolean             |optional  |true   |是否在 loading 的时候显示蒙层|
|showSearch      |boolean             |optional  |false  |是否显示内置的搜索栏| 
|headerHeight    |number              |optional  |40     |表格头部的高度|
|pageSize        |number              |optional  |10     |每页显示多少条数据|
|queryKeys       |array               |optional  |[]     |有哪些数据会传递给 subComp|
|jsxdata         |object              |optional  |-      |在远端数据还没有返回时用作默认数据|
|fetchUrl        |string              |optional  |""     |表格的数据源|
|fetchParams     |object              |optional  |-      |表格在请求数据时，会额外附带的参数，具有最高的优先级|
|actionBar       |object              |optional  |null   |表格内置的操作条配置|
|beforeFetch     |function(data, from)|optional  |noop   |两个参数，data 表示表格请求数据时即将发送的参数，from 表示这次请求数据的行为从哪里产生，内置的有 `search`(搜索栏),`order`(排序) & `pagination`(分页)，该函数需要返回值，返回值为真正请求所携带的参数。|
|processData     |function(data)      |optional  |noop   |有时源返回的数据格式，并不符合 Table 的要求，可以通过此函数进行调整，参数 data 是返回数据中 content 字段的 value，该函数需要返回值，返回值为符合 content 字段 value 的数据结构。|
|addRowClassName |function(rowData)   |optional  |noop   |用于为特定的某几行添加特殊的 class，用于样式定制|


### Tree 模式专用

|Name            |Type                |Require   |Default|Note |
|---             |---                 |---       |---    |---|
|renderModel     |string              |optional  |'tree' | render to tree model |
|levels          |number              |optional  |1      | tree model, default expand level number |

### 行内编辑表格专用

|Name            |Type                |Require   |Default|Note |
|---             |---                 |---       |---    |---|
|onChange        |function(data)      |option    |noop   |有表格编辑行为触发，参数的数据格式为 {data: 表格的所有数据, changedData: 变动行的数据, dataKey: xxx, editKey: xxx, pass: 正在编辑的域是否通过校验} |


### Props you should not define by yourself

> Parent will pass this props to his child  

|props name       |  defalut Value  |  Note   | 
|-----------      |  ------         |  -----    |
|passedData       |  null           |  Data passed from parent|



### Columns Config


|Key Name       |  Require  |  Type       | Note   | 
|-----------    |  ------   |  ---------- | -----  |
|dataKey        |  required |  string     | which key in data will be shown in view mode |
|editKey        |  optional |  string     | which key in data will be used in edit mode, equal to dataKey if not specified | 
|title          |  required |  string     | table head name |
|width          |  required |  number     |        |
|hidden         |  optional |  boolean    |        |
|order          |  optional |  boolean    | show the built-in sorter |
|type           |  optional |  string     | containing 'money', 'card', 'cnmobile', 'checkbox', 'action', 'radio', 'text', 'select' & 'custom' |
|actions        |  optional |  array      | when type =='action', we need this attr |
|customField    |  optional |  React Component| when type is 'custom', pass your custom Field extended from CellField to Table|
|render         |  optional |  function   | render the cell as you want, return a react element |
|fixed          |  optional |  boolean    | set the column fixed or not |
|delimiter      |  optional |  string     | delimiter used in type 'money', 'card', 'cnmobile' formating|
|align          |  optional |  string     | text-align, default: 'left' |
 

```javascript

let columns = [
        { dataKey: 'check', type: 'checkbox', disable: false}, // custom checkbox column, dataKey can be anyone, true means checked.
        { dataKey: 'country', title:'国家', width: 200,ordered:true},
        { dataKey: 'city',title:'城市', width: 150,ordered:true },
        { dataKey: 'firstName',title:"FristName" },  
        { dataKey: 'lastName' ,title:"LastName"},
        { dataKey: 'email',title:"Email",width: 200, ordered:true },
        { dataKey: 'action1', title:'操作1', width:100, type:"action",actions: [
            {
                title: '编辑',
                callback: (rowData) => {
                    me.refs.grid.editRow(rowData);
                },
                mode: Constants.MODE.VIEW
            },
            {
                title: '保存',
                callback: (rowData) => {
                    me.refs.grid.saveRow(rowData);
                },
                mode: Constants.MODE.EDIT
            }
        ]},
        { dataKey: 'action', title:'链接', width:100, render: function(cellData,rowData) {
            return <div><a href="#">{rowData.email}</a></div>
        }}
 ]

```



## Rules

* return data format [here](http://gitlab.alibaba-inc.com/alinw/yosemite/issues/18) 



```javascript
   {
	"content":{
		"data":[
			{	
				"id":'1'
				"grade":"grade1",
				"email":"email1",
				"firstName":"firstName1",
				"lastName":"lastName1",
				"birthDate":"birthDate1",
				"country":"country1",
				"city":"city1"
			}
			...
	
		],
		"currentPage":1,
		"totalCount":30
	},
	"success": true,
	"errorCode": "",
	"errorMsg": ""
	}

```

> the data format above is what server should send. If you pass data via jsxdata, you just need passed the `content`, like below

```javascript
{
    "data":[
        {   
            "id":'1'
            "grade":"grade1",
            "email":"email1",
            "firstName":"firstName1",
            "lastName":"lastName1",
            "birthDate":"birthDate1",
            "country":"country1",
            "city":"city1"
        }
        ...

    ],
    "currentPage":1,
    "totalCount":30
}
```

## API

### Row Editing 

* getData(): return cellData & do Validation
* addEmptyRow(): add an empty Row in 'edit' mode
* addRow(rowData): add an row with specified data in 'edit' mode.
* delRow(rowData): delete specified row by jsxid
* editRow(rowData): make the specified row in 'edit' mode.
* viewRow(rowData): make the specified row in 'view' mode.
* saveRow(rowData): save the row change.
* resetRow(rowData): cancel the row change before saveRow() is called.

### Data Fetching

* fetchData(from): call this method when you want the table to fetch Data via ajax again. 
    * @param from {string} {optional}: this param will be passed to props.beforeFetch.

### Other

* toggleSubComp(rowData): show or hide sub comp