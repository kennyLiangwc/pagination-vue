# vue 分页组件
	
	返回{pageSize: 1,pageNumber: 10}

## 用法
	1.在父组件引入pagination

```
	import Pagination from 'pagination'
```
2.传入total(数据总条数)、pageSize(每页显示条数)、pageNumber(当前页码)
```
	<Pagination :total=50 :pageSize=10 :pageNumber=1 />
```
3.emit pagechange
```
	<Pagination @pagechange="pagechange" />
```
