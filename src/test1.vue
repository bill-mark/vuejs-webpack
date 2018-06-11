<template>
<div class="center_wrap">
  <div class="top">
  	<div class="line1">
  		<allchose :initlist="brand" :initname="brand_label" :inittype="brand_name" @has_checked="get_selected"></allchose>
        <allchose :initlist="channel" :initname="channel_label" :inittype="channel_name" @has_checked="get_selected"></allchose>
  		<allchose :initlist="shop" :initname="shop_label" :inittype="shop_name" @has_checked="get_selected"></allchose>
  		<allchose :initlist="state_ac" :initname="state_ac_label" :inittype="state_ac_name" @has_checked="get_selected"></allchose>
  		<allchose :initlist="act_type" :initname="act_type_label" :inittype="act_type_name" @has_checked="get_selected"></allchose>
  		<allchose :initlist="show_type" :initname="show_type_label" :inittype="show_type_name" @has_checked="get_selected"></allchose>
  	</div>
  	<div class="line2">
  		<div class="title" >活动开始时间范围</div>
  		<div class="timeleft">
  			<el-date-picker
  			     size='small'
  			     v-model="starttime"
  			     type="datetimerange"
  			     :picker-options="pickerOptions"
  			     range-separator="至"
  			     start-placeholder="开始日期"
  			     end-placeholder="结束日期"
  			     align="right"
  			     value-format="yyyy-MM-dd HH:mm:ss"
  			     >
  			</el-date-picker>
  		</div>

  		<div class="title">活动结束时间范围</div>
  		<div class="timeleft">
  			<el-date-picker
  			     size='small'
  			     v-model="endtime"
  			     type="datetimerange"
  			     :picker-options="pickerOptions"
  			     range-separator="至"
  			     start-placeholder="开始日期"
  			     end-placeholder="结束日期"
  			     align="right"
  			     value-format="yyyy-MM-dd HH:mm:ss"
  			     >
  			</el-date-picker>
  		</div>
  		
  		<div class="title_right">
  			活动名称
  		</div>
  		<input class="right_input" v-model='activity_name'>
  	</div>
  	<div class="line3">
  		<div class="button" @click="post_selected_data">查询</div>
  		<div class="button" @click="go_add_act">添加</div>
  		<div class="button2">
  		   <div class="button2_title">上传</div>

  		   <input type="file" id="post_excel" class="button2_input" @change="importfxx" accept=".csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel">
  		</div>
  		<div class="button" @click="down_model">
  		     <a href="javascript:;" style="color:white">下载模板</a>
  		</div>
  		<div class="button" @click="down_excel">导出excel</div>
  	</div>
  	<div class="line4"></div>
  </div>


  <table class="middle_table">
  	<tr class="table_title">
  		<td>
  			<input class="list_first" type="checkbox"   @click="change_listState" v-model="list_state">
  			<div style="min-width:50px" class="first_linetd">序号</div>
  		</td>
  		<td>
  			<div style="min-width:120px" class="first_linetd">活动名称</div>
  		</td>
  		<td>
  			<div style="min-width:90px" class="first_linetd">品牌</div>
  		</td>
  		<td>
  			<div style="min-width:60px" class="first_linetd">渠道</div>
  		</td>
  		<td>
  			<div style="width:100px" class="first_linetd">店铺</div>
  		</td>
  		<td>
  			<div style="min-width:70px" class="first_linetd">活动状态</div>
  		</td>
  		<td>
  			<div style="min-width:70px" class="first_linetd">活动类型</div>
  		</td>
  		<td>
  			<div style="min-width:70px" class="first_linetd">是否展示</div>
  		</td>
  		<td>
  			<div style="min-width:90px" class="first_linetd">本场货值</div>
  		</td>
  		<td>
  			<div style="min-width:90px" class="first_linetd">均价</div>
  		</td>
  		<td>
  			<div style="min-width:90px" class="first_linetd">目标销额</div>
  		</td>
  		<td>
  			<div style="min-width:90px" class="first_linetd">销额完成率</div>
  		</td>
  		<td>
  			<div style="min-width:90px" class="first_linetd">目标加购人数</div>
  		</td>
  		<td>
  			<div style="min-width:70px" class="first_linetd">操作</div>
  		</td>
  	</tr>
  	<tr class="table_content" v-for="(item,index) in list_data">
  		<td>
  			<input class="list_first" type="checkbox" :id="item.id"  v-model="item.state">
  			<div style="width:50px;float:right">{{pageSize * (currentPage -1) +index +1}}</div>
  		</td>
  		<td>
  			<div style="width:120px;color:#2389ff;cursor:pointer" :title="item.act_name" @click="show_act(item.id)">
  			     {{item.act_name}}
  			</div>
  		</td>
  		<td>
  			<div style="width:90px">{{item.brand}}</div>
  		</td>
  		<td>
  			<div style="width:60px">{{item.channel}}</div>
  		</td>
  		<td>
  			<div style="width:100px;overflow:hidden" :title="item.shop_name">{{item.shop_name}}</div>
  		</td>
  		<td>
  			<div style="width:70px">{{item.state_ac | state_ac}}</div>
  		</td>
  		<td>
  			<div style="width:70px">{{item.act_type }}</div>
  		</td>
  		<td>
  			<div style="width:70px">{{item.delete | isshow}}</div>
  		</td>
  		<td>
  			<div style="width:90px">{{item.this_value}}</div>
  		</td>
  		<td>
  			<div style="width:90px">{{item.avg_price}}</div>
  		</td>
  		<td>
  			<div style="width:90px">{{item.target_sales}}</div>
  		</td>
  		<td>
  			<div style="width:90px">{{item.sale_rate}}</div>
  		</td>
  		<td>
  			<div style="width:70px">{{item.target_pl}}</div>
  		</td>
  		<td>
  			<div>
				<span class="caozuo" @click="go_editpage(item)">编辑</span>
				<span class="caozuo" @click="delet_window(false,item.id)">删除</span>
  			</div>
  		</td>
  	</tr>
  </table>

    <div class="batch" @click="delet_window(true)">批量删除</div>

   	<div class="pages" style="margin-top:30px;margin-left:50px;display:flex;justify-content:center">
  		  <el-pagination 
  		    @size-change="handleSizeChange"
  		    @current-change="handleCurrentChange"
  		    :current-page="currentPage"
  		    :page-sizes="[30, 40, 50]"
  		    :page-size=pageSize
  		    layout="total, sizes, prev, pager, next,slot,jumper"
  		    :total="list_total">
  		  </el-pagination>
   	</div>


  <!-- 全局弹窗组件_newalert -->
  <DialogAlert :dialogname="alert_title" :isShow="alert_status" @on-close="close_AlertWindow">
      <div style="padding: 10px 15px;;text-align:left;
                  font-size:14px;color:#606266">
         {{alert_text}}
      </div>
  </DialogAlert>

  <!-- 取消确认弹窗组件 -->
  <DialogConfirm  :isShow="con_alert_status" @on-close="con_close_AlertWindow" @besure="post_delet">
      <div style="padding: 20px 15px;;text-align:center;
                  font-size:16px;color:#606266">
         {{con_alert_text}}
      </div>
  </DialogConfirm>

  <!-- 查看活动详情组件 -->
  <ShowActivity :isShow="act_show_state" :item="act_data" @on-close="close_actWindow"></ShowActivity>

  <!-- 编辑活动 -->

</div>	
</template>
<script >
import allchose from '../components/all-chose.vue'
import XLSX from 'xlsx'
export default{
  components:{
    allchose,
    'DialogConfirm':()=> import('../components/confirmalert.vue'),
    'ShowActivity':()=> import('../components/show_act.vue')
  },
	data(){
		return{
			test:'7788',
			act_show_state:false,
			con_alert_status:false,
			con_alert_text:'',
			be_delet_id:[],
			be_delet_type:false,//false->单行删除,true->多行删除
			alert_title:'',
			alert_status:false,
			alert_text:'',
			activity_name:'',
			starttime:'',
			endtime:'',
			number_starttime:'',
			number_endtime:'',
			pickerOptions: {
		          shortcuts: [{
		            text: '最近一周',
		            onClick(picker) {
		              const end = new Date();
		              const start = new Date();
		              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
		              picker.$emit('pick', [start, end]);
		            }
		          }, {
		            text: '最近一个月',
		            onClick(picker) {
		              const end = new Date();
		              const start = new Date();
		              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
		              picker.$emit('pick', [start, end]);
		            }
		          }, {
		            text: '最近三个月',
		            onClick(picker) {
		              const end = new Date();
		              const start = new Date();
		              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
		              picker.$emit('pick', [start, end]);
		            }
		          }]
	    },
	    currentPage:1,//当前页
	    pageSize:30,//每页多少条
	    list_total:100,
	    list_state:false,//列表全选状态控制

			brand:[		                    
			],
			brand_label:'品牌',
			brand_name:'brand',

			channel:[
			],
			channel_label:'渠道',
			channel_name:'channel',

			shop:[
			],
			shop_label:'店铺',
			shop_name:'shop_name',

			state_ac:[
			],
			state_ac_label:'活动状态',
			state_ac_name:'state_ac',

			act_type:[
			],
			act_type_label:'活动类型',
			act_type_name:'act_type',

			show_type:[
			],
			show_type_label:'是否展示',
			show_type_name:'delete',

			list_data:[
			],

			check_filter:{  //查询参数
			   'brand':[],
               'channel':[],
               'shop_name':[],
               'state_ac':[],
               'act_type':[],
               'show_type':[],
               'act_name':''
			},
      act_data:{  //单条活动内容
		  },
      is_batch_delete:false,
		}
	},
	filters:{
      isshow:function(data){
      	if(data){
      		return '否'
      	}else{
      		return '是'
      	}
      },
      state_ac:function(data){
          switch(data)
          {
          	case '0':
          	    return '未开始'
          	    break;
          	case '1':
          	    return '预热中'
          	    break;
          	case '2':
          	    return '活动中'
          	    break;
          	case '3':
          	    return '已结束'
          	    break;
          	default:
          	    return data
          }
      }
	},
	mounted(){
       if(this.$route.params.check_filter != undefined){
          this.check_filter = this.$route.params.check_filter
          this.select_init()
          this.post_selected_data_simple()
          return
       }

       this.select_init()
       this.post_selected_data()
	},
	methods:{
      cc(){
         let c2 = [{'aa':13,'bb':false},{'aa':14,'bb':false}]
         console.log(c2[1].aa)
         console.log(c2[1].bb)
         c2[1].bb = true
         console.log(c2)
      },
      importfxx(event){ //获取本地excel
        var files = event.target.files;
        let that = this
        var fileReader = new FileReader();

        fileReader.onload = function(ev) {
          	 let formData = new FormData();
          	 formData.append('file', document.getElementById('post_excel').files[0])
             that.post_excel(formData)

             try {
                 var data = ev.target.result,
                     workbook = XLSX.read(data, {
                         type: 'binary'
                     }), // 以二进制流方式读取得到整份excel表格对象
                      persons = []; // 存储获取到的数据
             } catch (e) {
                 console.log('文件类型不正确');
                 return;
             }

             // 表格的表格范围，可用于判断表头是否数量是否正确
             var fromTo = '';
             // 遍历每张表读取
             for (var sheet in workbook.Sheets) {
                 if (workbook.Sheets.hasOwnProperty(sheet)) {
                     fromTo = workbook.Sheets[sheet]['!ref'];
                     persons = persons.concat(XLSX.utils.sheet_to_json(workbook.Sheets[sheet]));
                 }
             }
        };
        // 以二进制方式打开文件
        fileReader.readAsBinaryString(files[0]);            
      },
      post_excel(persons){  //上传Excel
              this.$http.post('/v1.0/assemble/action/upload', persons)
      				.then((res)=>{
                   if(res.data.status == 200){
                        this.alert_status = true
                        this.alert_title = "提示"
                         this.alert_text = '上传成功'
                   }else{
                    console.log('res')
                    console.log(res)
                    this.alert_status = true
                    this.alert_title = "上传Excel接口出错1"
                    this.alert_text = error.message  
                   }
              })
              .catch( (error)=>{
                   console.log('error')
                   console.log(error)
                   console.log(error.response)
                   this.alert_status = true
                   this.alert_title = "上传Excel出错"
                   this.alert_text = error.response.data.message              
              })
      },
      down_excel(){ //Excel下载
        this.check_filter.act_name = this.activity_name
      	this.check_filter.start_date_range = this.starttime
      	this.check_filter.end_date_range = this.endtime

          this.$http({
            method:'post',
            url:'/v1.0/assemble/action/excel/download',
            data:{
                page:this.currentPage,
                num:this.pageSize,
                filter:this.check_filter 
            },
            responseType:'arraybuffer',
            headers:{'filename':'utf-8'}
          })
          .then( (res)=>{
             　let blob = new Blob([res.data], {type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"}); 
               let objectUrl = URL.createObjectURL(blob); 

               let fileName = encodeURI(encodeURI (res.headers['content-disposition'].split('=')[1] ))
                if (window.navigator.msSaveOrOpenBlob) {
                    navigator.msSaveBlob(blob, fileName);
                } else {
                    var link = document.createElement('a');
                    link.href = window.URL.createObjectURL(blob);
                    link.download = fileName;
                    link.click();
                    window.URL.revokeObjectURL(link.href);
                }              
          })
          .catch( (error)=>{
            this.alert_status = true
            this.alert_title = '警告'
            this.alert_text = '下载接口出错'
          })
      },
      down_model(){  //模板下载
           window.location.href="/v1.0/assemble/action/excel/template/download"
      },
      change_listState(){  //列表全选
          console.log('列表全选')
          if(!this.list_state){
             this.list_state = true
             this.list_data.map( (v)=>{
             	  v.state = true
             })

          }else{
          	this.list_data.forEach( v=>{
             	  v.state = false
              })
          	this.list_state = false
          }
      },
      format_time(data){  //时间格式化
           if(data == ''|| data == null){
           	return []
           }
      	 let result = []
      	 data.forEach( v=>{
      	 	let date = this.format_datemonth(v.getFullYear()+"-"+(v.getMonth()+1)+"-"+v.getDate())
      	 	let time = this.starttime[0].toTimeString().split(" ",1)[0]
      	 	result.push(date+' '+time)
      	 })
      	 return result
      },
      format_datemonth(date){  //日期补0
            let f2 = ''
            let f3 = ''

            if( date.split('-')[1]  < 10   ){
               f2 = '0'+date.split('-')[1]               
            }else{
               f2 = date.split('-')[1] 
            }

            if( date.split('-')[2]  < 10   ){
               f3 = '0'+date.split('-')[2]              
            }else{
               f3 = date.split('-')[2] 
            }

            return date.split('-')[0] +'-'+ f2+'-'+f3
      },
  		get_selected(data,type){  //获得子组件筛选结果
  			//console.log('get_selected')
  			//console.log(type)
  			this.check_filter[type] = data
  		},
  		close_AlertWindow(){  //全局弹窗组件
  		    this.alert_status = false
  		},
  		con_close_AlertWindow(){  //确认弹窗组件
              this.con_alert_status = false
  		},
  		con_besure(){  //确认弹窗确认按钮

  		},
      select_init(){  //选项初始化
  			this.$http.get('/v1.0/assemble/filters')
  			.then((res)=>{
  			       let result = res.data.data.filters
  			       result.forEach(v=>{
  			       	  v.values[0].last_state = false
  			       })
  			       this.brand = result[0].values
  			       this.channel = result[1].values
  			       this.shop = result[2].values
  			       this.state_ac = result[3].values
  			       this.act_type = result[4].values
  			       this.show_type = result[5].values
  			})
  			.catch((error)=> {
  				   console.log(error)
  			       this.alert_status = true
  			       this.alert_title = "初始化选项接口错误"
  			       this.alert_text = error.message
  			});
      },

      post_selected_data(){  //提交查询请求
      	  this.check_filter.act_name = this.activity_name
          this.check_filter.start_date_range = this.starttime
          this.check_filter.end_date_range = this.endtime

          if(this.is_batch_delete){
                       this.currentPage = 1
          }

           this.$http.post('/v1.0/assemble/action/search',{             
                page:this.currentPage,
                num:this.pageSize,
                filter:this.check_filter
           })
           .then((res)=>{
                if(this.is_batch_delete){
                       this.is_batch_delete = false
                 }
                this.list_total = res.data.data.total_count
                res.data.data.acts.forEach( v=>{
                  v.state = false
                })
                this.list_data = res.data.data.acts
           })
           .catch( (error)=>{
                this.alert_status = true
                this.alert_title = "初始化选项接口错误"
                this.alert_text = error.message               
           })
      },
      post_selected_data_simple(){  //提交查询请求
           this.$http.post('/v1.0/assemble/action/search',{             
                page:this.currentPage,
                num:this.pageSize,
                filter:this.check_filter
           })
           .then((res)=>{
                this.list_total = res.data.data.total_count
                this.list_data = res.data.data.acts
           })
           .catch( (error)=>{
                this.alert_status = true
                this.alert_title = "初始化选项接口错误"
                this.alert_text = error.message               
           })
      },
  
      handleSizeChange(val) { //更改每页展示条数
	        	this.pageSize = val;
	    	    this.post_selected_data();
	    },
	    handleCurrentChange(val) { //分页回调函数
	    	    this.currentPage = val;
	    	    this.post_selected_data();
	    },
      delet_window(type,id){   //删除弹窗
             let test_1 = this.list_data.filter( (item,index)=>{
                  return item.state == true
             })
             if( (type == true) && (test_1.length == 0) ){
                  this.alert_status = true
                  this.alert_title = "提示"
                  this.alert_text = "没有活动被选中"
                  return
             }

             this.con_alert_status = true
             this.con_alert_text = "一旦删除，数据将不可恢复。是否确认删除?"

    
             this.be_delet_type = type
             this.be_delet_id = []

             if(!type){
             	  this.be_delet_id.push(id)
             }else{
                this.is_batch_delete = true
               	this.list_data.forEach( v=>{
                 		if(v.state){
                 			this.be_delet_id.push(v.id)
                 		}
               	})
             }          
      },
	    post_delet(){  //提交删除
	           this.con_alert_status = false
             this.$http.post('/v1.0/assemble/action/delete',{             
                  id:this.be_delet_id
             })
             .then((res)=>{
                  if(res.data.status == 200){
                  	 this.alert_status = true
                  	 this.alert_title = "提示"
                  	 this.alert_text = "删除成功!"
 
                     if(this.is_batch_delete){
                       this.list_state = false
                     }
                     

                  	 this.post_selected_data();
                  }
             })
             .catch( (error)=>{
                  this.alert_status = true
                  this.alert_title = "删除接口错误"
                  this.alert_text = error.message               
             })
	    },
	    close_actWindow(){  //关闭查看活动详情窗口
           this.act_show_state = false
	    },
	    show_act(id){  //查看活动详情       
           this.$http.get('v1.0/assemble/action/check',{
           	  params:{
           	  	id:id,
           	  }
           })
           .then((res)=>{
                  if(res.data.status == 200){
                  	this.act_show_state = true
                  	this.act_data = res.data.data.act
                  }else{
                  	 this.alert_status = true
                  	 this.alert_title = "查看活动数据接口错误"
                  	 this.alert_text = error.message
                  }
           })
           .catch((error)=> {
                  this.alert_status = true
                  this.alert_title = "查看活动数据接口错误"
                  this.alert_text = error.message
           });
	    },
	    go_add_act(){  //跳转添加活动页面
            window.open(window.location.origin + '/#/add')
	    },
	    go_editpage(data){  //跳转编辑活动页面
            //localStorage.check_filter = JSON.stringify(this.check_filter)
           // console.log('跳转编辑活动页面')
            //console.log(this.check_filter)
            this.$router.push({name:'edit_activity',params:{'data':data,'check_filter':this.check_filter} })
	    }
	}
}
</script>
<style lang="scss" >
.center_wrap{
	min-width:1200px;
	.top{
		.line1{
		      display: flex
		}
		.line2{
			  margin-top: 30px;
			  display: flex;
			  .title{
			  	margin-left: 20px;
			  	padding-top: 5px;
			  	margin-right: 15px;
			  }
			  .timeleft{
			      .el-input__inner{
			      	height: 28px;
			      	width: 330px;
			      }
			      .el-range__icon{
			      	line-height: 21px;
			      	margin-left: -6px;
			      	padding-right: 3px;
			      }
			      .el-range-input{
			      	width: 134px;
			      }
			      .el-range-separator{
			      	width: 20px;
			      	margin-top: -6px;
			      }
			      .el-range__close-icon{
			      	margin-left: 3px;
			      	margin-top: -2px;
			      }
			  }
			  .title_right{
			  	 margin-left: 30px;
			  	 padding-top: 5px;
			  	 margin-right: 15px;
			  }
			  .right_input{
			  	    height: 27px;
			  	    border-radius: 4px;
			  	    padding-left: 10px;
			  	    width: 129px;
			  }
		}
		.line3{
			  display: flex;
			  .button{
			  	margin-top: 20px;
			  	margin-left: 20px;
			  	font-size: 13px;
			  	color: #fff;
			  	line-height: 24px;
			  	cursor: pointer;
			  	text-align: center;
			  	background: #2389ff;
			  	border-radius: 4px;
			  	width: 60px;
			  	height: 24px;
			  }
			  .button2{
			  	
			  	margin-top: 20px;
			  	margin-left: 20px;
			  	font-size: 13px;
			  	color: #fff;
			  	line-height: 24px;
			  	cursor: pointer;
			  	text-align: center;
			  	background: #2389ff;
			  	border-radius: 4px;
			  	width: 60px;
			  	height: 24px;

			  	position:relative;
			  	.button2_title{
                   position: absolute;
                   left: 9px;
                   top: 0px;
                   width: 35px;
                   height: 1px;
			  	}
			  	.button2_input{
                  width: 59px;
                  margin-left: 1px;
                  opacity: 0;
                  z-index: 99;
                  cursor: pointer;
			  	}
			  }
		}
		.line4{
			  margin-top: 20px;
			  margin-left: 20px;
			  border-top:1px solid #666;
		}
	}
    .middle_table{
    	margin-top:20px;
    	margin-left: 20px;
    	.table_title{
			width: 957px;
			font-size:12px;
			color:#999999;
			background:#f3f5fa;
			.first_linetd{
			 color:#666;
			 height:30px;
			 font-size:12px;
			 text-align:center;
			 line-height:30px;
			}
		}
		.table_content_title:hover{
			cursor: pointer;
			color:#2389ff;
		}
		.table_content td div{
			text-align:center;
			font-size:12px;
			color:#666;
			height: 30px;
			line-height: 30px;
			overflow: auto;
		}
		.list_first{
			float: left;
			margin-top: 7px;
			margin-left: 9px;
		}
		.caozuo{
			color:#2389ff;
			cursor: pointer;
			font-size: 12px;
			margin-left: 5px;
		}
    }
    .middle_table td{
    	border:1px solid #e5e5e5;
    }
    .batch{
    	    width: 53px;
    		color:#2389ff;
    		cursor: pointer;
    		font-size: 12px;
    		margin-left: 20px;
            margin-top: 10px;
    }
}	
</style>
















