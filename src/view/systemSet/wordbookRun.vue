<template>
  <div class="warp">
    <div class="xd_top">
      <div class="addbtn">
        <el-button
          type="primary"
          icon="el-icon-plus"
          @click="addData"
          size="small"
           v-if="hasPerm('pram:insert')"
          >新增</el-button
        >
      </div>
       <div class="addbtn">
        <label>参数名称：</label>
            <el-input
            v-model="name"
            label="参数名称"
            placeholder="请输入参数名称"
            style="width:200px"
          ></el-input>
      </div>
       <div class="addbtn">
         <label>参数分类：</label>
          <el-input
            v-model="type"
            label="参数分类"
            placeholder="请输入参数分类"
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
          <template slot-scope="scope">
                  <span>{{scope.$index + 1}}</span>
              </template>
        </el-table-column>
        <el-table-column prop="name" label="参数名称"> </el-table-column>
        <el-table-column prop="type" label="参数分类" show-overflow-tooltip>
        </el-table-column>
        <el-table-column prop="key" label="参数键" show-overflow-tooltip>
        </el-table-column>
        <el-table-column prop="value" label="参数值" show-overflow-tooltip>
        </el-table-column>
        <el-table-column label="创建时间" prop="createDate" show-overflow-tooltip>
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
              size="mini"
              type="text"
               v-if="hasPerm('pram:update')"
              @click="handleDetails(scope.$index, scope.row)"
              >编辑</el-button
            >
            <el-button
              size="mini"
              type="text"
              v-if="hasPerm('pram:delte')"
              @click="handleDel(scope.$index, scope.row)"
              >删除</el-button
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
        :page-sizes="[5, 10, 50, 100]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import {wordgetPage,wordDel,wordgetAll} from "../../api"
export default {
  data() {
    return {
      currentPage: 1,
      pageSize: 10,
      total: 0,
      name: "",
      type:'',
      inputName: "",
      tableData: [

      ],
    };
  },
  created(){
    this.init()
  },
  methods: {
    init(){
      wordgetPage({
        page:this.currentPage,
        limit:this.pageSize
      }).then(res => {
        this.tableData = res.data.data.rows
        this.total =res.data.data.total
      })
    },
    onSearch() {
      //搜索
      wordgetPage({
        name:this.name,
        type:this.type,
        page:this.currentPage,
        limit:this.pageSize
      }).then(res => {
        this.tableData = res.data.data.rows
        this.total =res.data.data.total
      })
    },
    handleDetails(index,row) {
      //编辑
      this.$router.push({
        name:'wordbookRundetalis',
        query:{
          id:row.id
        }
      })
    },
    handleDel(index,row){
      this.$confirm("此操作将永久删除该文件, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          wordDel({
            id:row.id
          }).then((res) => {
            this.init()
          });
          this.$message({
            type: "success",
            message: "删除成功!",
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除!",
          });
        });
    },
    addData() {
      //新增
      this.$router.push({
        name:'wordbookRunadd'
      })
    },
    handleSelectionChange(val) {
    
      this.multipleSelection = val;
    },
    handleSizeChange (val) {

       this.pageSize = val;
        this.init()
    },
    handleCurrentChange (val) {
       this.currentPage = val;
        this.init()

    }
  },
};
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
