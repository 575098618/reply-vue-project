<template>
	<div id="loadingOrder" class="u-load" v-if="loading"><em class="icn-load"></em>努力加载中...</div>
    <div class="table-responsive" v-else>
	    <table class="u-table2">
	    	<tr>
	    		<td colspan="7" style="font-weight: bold;">学生列表</td>
	    	</tr>
	    	<tr>
	    		<td>
	    			姓名
	    		</td>
	    		<td>
					学号
				</td>
	    		<td>
	    			所在学院
	    		</td>
				<td>
					答辩小组
				</td>
				<td>
					通过票数
				</td>
				<td>
					不通过票数
				</td>
				<td>
					答辩状态
				</td>
	    	</tr>
	    	<tbody v-for="i in size ">
	    		<tr>
	    			<td>
	    				{{studentList[i + start].username}}
		    		</td>
		    		<td>
		    			{{studentList[i + start].number}}
		    		</td>
		    		<td>
						{{studentList[i + start].school}}
					</td>
					<td>
						{{studentList[i + start].groupid}}
					</td>
					<td>
						{{studentList[i + start].pass_count}}
					</td>
					<td>
						{{studentList[i + start].reject_count}}
					</td>
					<td>
						{{studentList[i + start].status}}
					</td>

	    		</tr>
	    	</tbody>
	    	 <tr>
				 <td colspan="6"></td>
			 </tr>
	    	<tr>
	    		<td colspan="2" style="font-weight: bold;">总计</td>
	    		<td>{{total}}</td>
	    	</tr>
	    </table>


		<pagination :query.sync="pager"></pagination>
	</div>


</template>

<script>
	
    module.exports = {

        data: function () {
			return {
				pager: {start: 1, limit: 10, total: 0},
				size: 0,
				start: 0,
				page_num: 1,
				loading: false,
				studentList:{}
			}
        },
		events: {
//          /** 分页中的跳页  */
			"jump": function (index) {
				console.log("--------jump--------------");
				var $this = this;
				$this.page_num = index;
				$this.page(index);
			},
		},
        methods: {

			page: function(index) {
				var $this = this;
				$this.loading = true;
					setTimeout(function() {
						console.log("set time out");
						$this.loading = false;
					},1000);
				 $this.$am.ajax({
				 url: $ApiConf.api_student_list,
					 success: function (data) {
					$this.studentList=data.studentList;
						 var dq = $this.studentList;
						var total = dq.length;
						var start = (index - 1) * 10;
						if((total - start) < 10) {
							$this.size = total - start;
						}else {
							$this.size = 10;
						}
						$this.$set("total",total);
						$this.start = start;

						$this.pager.total = total;
						$this.pager.start = index;


					 }
				 });

			},

        },
        route: {
            activate: function (transition) {
            	console.log("dq activate");
				var $this = this;
            	document.title = $this.$route.menuName + "-小米金融";
				//$this.page(1);
                /**
                 * 权限控制
                 */
                transition.next();
            },
        },
		ready: function() {
			var $this = this;
			console.log(" ready ");
			$this.page(1);
		},

	}
</script>
