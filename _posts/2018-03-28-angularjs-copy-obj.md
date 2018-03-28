---
layout: post
title:  "AngularJs对象复制问题!"
date:   2018-03-28 11:14:54
category: AngularJs
tags: [AngularJs]
---
{% include JB/setup %}
*在AngularJs中不可避免的会使用到数组遍历的功能，之后对遍历后的数据做修改。*

列表循环示例：

```
<tr ng-show="rows.length>0" ng-repeat="row in rows">
	<td><input type="checkbox" grid-selection grid-selection-id="userId" grid-selection-row="row" ng-model="row.selected"></td>
		<td class="text-ellipsis">
			<p class="small mb0">{{row.userName}}</p>
		</td>
		<td class="text-ellipsis">
			<p class="small mb0">{{row.certificateTypeNameArr}}</p>
			<p ng-if="row.certificateSerNum" class="small mb0">培训编号({{row.certificateSerNum}})</p>
			<p ng-if="row.hasCer" class="small mb0">{{row.hasCer == 1?'证书':'培训记录'}}</p>
		</td>
		<td class="text-ellipsis" >
			<p class="small mb0">{{row.endScore}}</p>
		</td>
		<td class="text-ellipsis">
			<p ng-class="row.orderState==1?'small mb0 text-success':'small mb0 text-danger'">
									{{row.orderType==1?'个人':(row.orderType==2?'单位':'')}}{{row.orderState==1?'已缴费':'未交费'}}
			</p>
		</td>
		<td class="text-ellipsis">
			<p class="small mb0">{{row.passState=='1'?'通过':''}}{{row.passState=='2'?'未通过':''}}</p>
		</td>
		<td class="text-ellipsis">
			<p class="small mb0">{{row.passTime | date:'yyyy-MM-dd HH:mm:ss'}}</p>
		</td>
		<td class="text-ellipsis">
			<p class="small mb0">{{row.reportState=='1'?'已报到':'未报到'}}</p>
		</td>
		<td class="text-center">
			<table-operation options="tableOpts" row="row"></table-operation>
		</td>
	</tr>		
```
在最后一列的操作列，增加修改方法，将row传递给详情model。
详情示例：

```
$scope.tableOpts = {
    "cols" : [{
    	   "operate" : "classstu.edit_score",
    	   "operateText":"学习结果",
    	   "operateIcon":"glyphicon glyphicon-edit",
    	   "isHidden" :function(row){
	        	return $scope.classDetailInfo.classState==4 && row.passState=='1';
	        },
    	   "event":function(row){
    		   $scope.preUpdateScore(row);
    	   }
    	}]
}
	

$scope.preUpdateScore = function(row){

    $scope.classStu = row;
}
```
在preUpdateScore方法内对$scope.classStu中的属性做修改，突然发现row内的属性也随之变更了。

原因：
> 在JS中对变量赋值变量时，常规是会复制一份拷贝，但如果值是一个对象（Object）时，传入的将是对象的地址。

处理方法
> 使用$scope.classStu = angular.copy(row);的方式即可