<template>
  <div>
    <el-page-header @back="goBack" content="编辑"> </el-page-header>
    <div class="tray">
      <el-form
        ref="orgFrom"
        :rules="rules"
        :model="orgFrom"
        label-width="100px"
      >
        <el-form-item label="机构名称：" prop="name">
          <el-input v-model="orgFrom.name"></el-input>
        </el-form-item>
        <el-form-item label="上级ID：" prop="parentId">
          <el-select v-model="orgFrom.id" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.id"
              :label="item.name"
              :value="item.id"
            >
            </el-option>
          </el-select>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" size="small" @click="onSubmit('orgFrom')"
            >确认</el-button
          >
          <el-button size="small" @click="goBack">取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { orgEdit, orgEditSubmit, orggetAll } from "../../../api";
export default {
  data() {
    return {
      orgFrom: {
        name: "",
        id: "",
      },
      options: [],
      rules: {
        name: [{ required: true, message: "请输入机构名称", trigger: "blur" }],
        id: [
          { required: true, message: "请选择上级Id", trigger: "change" },
        ],
      },
    };
  },
  created() {
    this.getIdData();
    this.getSelectData();
  },
  methods: {
    getIdData() {
      orgEdit(this.$route.query.id).then((res) => {
        (this.orgFrom.name = res.data.data.name),
          (this.orgFrom.id = String(res.data.data.parentId));
      });
    },
    getSelectData() {
      orggetAll().then((res) => {

        this.options = res.data.data;
      });
    },
    goBack() {
      this.$router.go(-1);
    },
    onSubmit(orgFrom) {
      this.$refs[orgFrom].validate((valid) => {
        if (valid) {
          orgEditSubmit({
            id: this.$route.query.id,
            name: this.orgFrom.name,
            parentId: this.orgFrom.id,
          }).then((res) => {
            this.$router.go(-1);
            this.$message({
              message: "修改成功",
              type: "success",
            });
          });
        } else {
          return false;
        }
      });
    },
  },
};
</script>

<style>
.tray {
  width: 50%;
  margin: 24px auto 0;
}
</style>