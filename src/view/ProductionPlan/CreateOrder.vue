<template>
  <div class="warp">
    <div class="xd_top">
      <div class="addbtn">
        <el-button
          type="primary"
          icon="el-icon-plus"
          @click="addData"
          size="small"
          v-if="hasPerm('notice:insert')"
          >新增</el-button
        >
      </div>
        <!--<div class="addbtn">
        <label>通知单：</label>
            <el-date-picker
      v-model="createDate"
      type="monthrange"
      range-separator="至"
      value-format="yyyy-MM-dd"
      start-placeholder="开始月份"
      end-placeholder="结束月份">
    </el-date-picker>
      </div>-->
      <div class="addbtn">
        <label>通知单名称：</label>
             <el-input
            v-model="name"
            placeholder="请输入"
            style="width:200px"
          ></el-input>
      </div>

      <div class="addbtn">
         <el-button
          type="primary"
          @click="onSearch"
          size="small"
          >查询</el-button
        >
      </div>
    </div>
    <div class="xd_table">
      <el-table
        ref="multipleTable"
        :data="tableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange"
      >
        <el-table-column label="序号">
          <template slot-scope="scope"><span>{{scope.$index + 1}}</span></template>
        </el-table-column>
        <el-table-column prop="name" label="通知单名称"> </el-table-column>
        <!--<el-table-column prop="address" label="通知单编号" show-overflow-tooltip>
        </el-table-column>-->
       <!-- <el-table-column prop="productNum" label="产品数量" show-overflow-tooltip>
        </el-table-column>
        <el-table-column prop="farmName" label="生产车间" show-overflow-tooltip>
        </el-table-column>
        <el-table-column prop="dutyUserName" label="负责人" show-overflow-tooltip>
        </el-table-column>-->
        <el-table-column prop="createDate" label="创建时间" show-overflow-tooltip>
        </el-table-column>
       <!-- <el-table-column prop="state" label="完成情况" show-overflow-tooltip>
        </el-table-column>-->
        <el-table-column label="操作">

          <template slot-scope="scope">
            <el-button
              size="mini"
              type="text"
              v-if="hasPerm('notice:update')"
              @click="handleEdit(scope.$index, scope.row)"
              >编辑通知单</el-button
            >

          </template>

        </el-table-column>
      </el-table>
    </div>
    <div class="block">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-sizes="[10, 50, 100, 200]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="pageCount"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import {noticegetPage,noticegetAll} from "../../api"
export default {
  data () {
    return {
        currentPage: 1,//当前页
      pageSize: 10, //每页显示条数
      pageCount: 0, //总数
      name:'',
      createDate:"",
      tableData: [

      ],

    }
  },
  created(){
    this.init()
  },
  methods: {
    init(){
      noticegetPage({
       page:this.currentPage,
        limit:this.pageSize
      }).then(res => {
       
        this.tableData =res.data.data.rows
        this.pageCount = res.data.data.total
      })
    },
    onExport () {
      // 导出
      

    },
    onSearch(){
      noticegetPage({
        name:this.name,
        page:this.currentPage,
        limit:this.pageSize
      }).then(res => {
       
        this.tableData =res.data.data.rows
        this.pageCount = res.data.data.total
      })
    },
    handleEdit (index,row) {
      // 编辑
      this.$router.push({
        name:'CreateOrderdetails',
        query:{
          id: row.id
        },
      })
    },

    addData () {
      // 新增
      this.$router.push({
        name:'CreateOrderadd'
      })
    },
    handleSelectionChange (val) {
   
      this.multipleSelection = val
    },
    handleSizeChange (val) {

       this.pageSize = val;
        this.init()
    },
    handleCurrentChange (val) {
       this.currentPage = val;
        this.init()

    }
  }
}
</script>

<style>


.xd_top {
  display: flex;
}
.addbtn {
  margin-right: 20px;
}
.xd_table {
  margin-top: 15px;
}
.block {
  width: 100%;
  margin-top: 15px;
  text-align: right;

}
.lable_input {
  display: flex;
  margin-right: 15px;
}
/* .lable_input label {
  display: inline-block;
  width:100px;
  line-height: 3;
} */
.search{
  display: flex;
}
</style>
