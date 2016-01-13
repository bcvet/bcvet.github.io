##使用名词而不是动词
	比如 /user    /user/12
	不要使用 getAll、delById
	一些非名词所能形象化表达的可以使用动词比如import、publish、search等
##使用子资源表达关系
  	rest/a/project/annountcement/{projectId}/list        查看项目公告列表,该项目为{projectId} 
  	rest/a/project/{projectId}/annountcement/list      查看项目{projectId}下的公告列表 --- 此表达更形象
##跳转页面
	/user/index                   ---index()、main()//跳转列表页、主页面
	/user/{id}/view               ---view()或者show() //跳转查看页  
	/user/form                    ---form()  //跳转新增页面             
	/user/{id}/edit               ---edit()   //跳转编辑页面
##不添加PUT DELETE  HTTP动词的请求(目前使用这种方式)
	增加C
	/user/save                  ----save()、create()、insert()//新增   POST
	/user/batch	      	   		----batchSave()//批量新增             POST
	查找R
	/user/list                ---list()          //列表分页查询        GET
	/user/{id}/               ---select()、read()      //获取资源信息   GET
	删除D
	/user/{id}/delete         ---delete() //删除                   GET/POST  
	/user/{id}/batchDelete    ---batchDelete()//批量删除				POST
	修改U
	/user/{id}/update         ---update()//修改					   GET/POST
	/user/{id}/batchUpdate    ---batch()//批量修改					POST
##以后添加PUT  DELETE  HTTP动词的请求
	/user                              GET      ----list()                  查--列表
	/user/{id}                         GET      ----select()、read()         查
	/user                              POST    ----save()\create()\insert   增
	/user/batch                        POST     ----batchSave()			     增--批量  project/{projectId}/users/batch
	/user/{id}                         PUT       ----update()               改
	/user/batch                        PUT       ----batchUpdate()          改--批量
	/user/{id}                         DELETE ----delete()                  删  
	/user/batch                        DELETE ----batchDelete() 			 	删--批量
	
	PUT和DELETE的前台请求demo
	var ids = new Array();
	ids.push("1");ids.push("2");ids.push("3");
	$.ajax({
		url:base+"/rest/restfulDemo/batch.json",
		type:'POST',
		dataType: "json",
		data:{
			_method:'DELETE',//也可以小写，建议大写为准
			ids:"",
			name:'3123'
		},
		success:function(data){
			bootbox.alertDialog("删除成功");
		}
	});
	后台Controller,接收
	@RequestMapping(value="/batch",method=RequestMethod.DELETE)
	如果_method:'PUT',前台如果表单序列化，需要<input type="hidden" name="_method" value="PUT"/>
	则对应@RequestMapping(value="/batch",method=RequestMethod.PUT)
	请见RestfullDemoController;
##url样例
	url:user/1/teacher
	*GET  返回与user的id是1有关联的teacher实例
	*POST 创建一个与user的id是1有关联的teacher实例
	url:user/1/teacher/1
	*DELETE 删除一个与user的id1有关联的teacher1的内容	
	*PUT    修改一个与user的id1有关联的teacher1的内容
	url:user/1/teacher/1/student
	*GET  返回与user1和teacher1有关联的student实例
	*POST 创建一个与user1和teacher1有关联的student实例
##ajax请求,组装复杂数据
	目前主要分三种

	* data:  
   	{

		content:'',//评论内容;
		projectId:'',//项目id;
		mainId:'',//主题id;
		projectCourseId:'',//项目课程id;
		mainType:'',//主题类型;
		level:1,//1 发表评论都是1级评论，level为1;
		createUser:'',//发表人的id;
		roleType:''//角色类型;
   	}
	* data:$("#askwork").serializeJSON();// 将form表单中的数据序列化为js对象----大多数使用
	*  var meeting = {};
		meeting.type = $("#meetingRadio input:checked").val();
		meeting.resourceId = $("#resourceId").val();
		meeting.id = $("#id").val();
		meeting.title = $("#title").val();
		meeting.url = $("#yyUrl input").val();
		data:meeting
	统一规范,建议如下
	var data = $("#askwork").serializeJSON();//form表单
	var data = {
		'csList' : csList,
		name : name,
		......
	}
	data:data 

	目前使用的组装对象：  /js/common/jquery.serializeJson.js             			serializeJson()
	目前使用的组装字符串：  /plug-in/jquery/jquery.serialize-object.js            	serializeJSON()
	统一规范：/plug-in/jquery/jquery.serialize-object.js     serializeObject() 序列化成对象  serializeJSON() 序列化成字符串
##后台拼装返回的json数据
	目前返回的数据有以下几种方式:
	1.通过map返回数据,map包含单个对象或者多个对象和参数的集合
	2.通过setAttributes方法组装map返回信息,比如setAttributes(map)
	3.通过setObj方法组装map返回信息,比如setObj(map)
	4.通过setObj方法返回单个对象或者参数信息,
	5.通过setMsg方法返回单个信息，比如setMsg(count)  setMsg(type)
	统一规范：
	1.返回单个对象或者单个参数通过setObj(),不用组装到map返回
	  AjaxJson j = new AjaxJson(true, "成功！");
	  j.setObj(obj);或者j.setObj(type);
    2.返回多个对象和参数通过setObj组装map返回
	  Map map = Maps.newHashMap();
	  map.put("obj",obj);
	  map.put("userId",userId);
	  AjaxJson j = new AjaxJson(true, "成功！");
	  j.setObj(map);
	3.msg放置成功或者失败信息或者其他向前台返回的提示信息
##请求成功的json返回值格式标准 （目前是通过AjaxJson组装返回信息，所以使用固定的AjaxJson属性格式即可）
	{
	    "obj": 
		{
        	"id": "402893954fac708b014fac708b780000",
        	"userName": "15053137313",
        	"status": 0,
        	"password": "f9b9a158b0f57bdbf2cb0c859493904a385dfba5692361d6af3cf7cd",
        	"token": {
            	"accessToken": "402816815214eaa9015214f17cd10008",
            	"expireDate": 1452057516497
			}
         },
	    "success": true,
	    "msg": "处理成功！"
	}
##请求失败的json返回值格式标准
	{
    	"attributes": null,
    	"obj": null,
    	"success": false,
    	"msg": "处理失败！"
	}
##分页的json返回值格式标准
	{
		"params":{
		
					"name":null,
					"isNeedGroup":null,
					"updateUser":null,
					......
		},
		"totalPage":1,
		"pageSize":10,
		"results":[
			{
				"name":"测试项目-cy_1",
				"endTime":null,
				"startTime":1436184585000
				......
			},
		    {	
				"name":"公共测试项目",
				"endTime":null,
				"startTime":1436185493000
				......
			},
		    {
				"name":"周圆正的项目",
				"endTime":null,
				"startTime":1436439860000
			}
		],
		"pageNo":1,
		"order":"3",
		"success":true,
		"msg":"处理成功！",
		"totalRecord":8
	}
