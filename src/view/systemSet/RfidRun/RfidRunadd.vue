<template>
  <div>
    <el-page-header @back="goBack" content="新增"> </el-page-header>
    <div class="tray">
      <el-form
        ref="rfidFrom"
        :rules="rules"
        :model="rfidFrom"
        label-width="100px"
      >
        <el-form-item label="RFID编码：" prop="rfid">
          <el-input v-model="rfidFrom.rfid"></el-input>
        </el-form-item>
        <el-form-item label="epc：" prop="epc">
          <el-input v-model="rfidFrom.epc"></el-input>
        </el-form-item>
       <!-- <el-form-item label="状态：">
          <el-select v-model="rfidFrom.state" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            >
            </el-option>
          </el-select>
        </el-form-item>   -->
        <el-form-item>
          <el-button type="primary" size="small" @click="onSubmit('rfidFrom')"
            >确认</el-button
          >
          <el-button size="small" @click="goBack">取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { rfidInsert } from "../../../api";
export default {
  data() {
    return {
      rfidFrom: {
        epc: "",
        rfid: "",
         state: "",
      },
      rules: {
        epc: [{ required: true, message: "请输入epc", trigger: "blur" }],
        rfid: [{ required: true, message: "请输入rfid", trigger: "blur" }],
      },
      options: [
        {
          value: 0,
          label: "未使用",
        },
        {
          value: 1,
          label: "已使用",
        },
      ],

    };
  },
  methods: {
    goBack() {
      this.$router.go(-1);
    },

    onSubmit(rfidFrom) {
      this.$refs[rfidFrom].validate((valid) => {
        if (valid) {
          rfidInsert({
            rfid: this.rfidFrom.rfid,
            epc: this.rfidFrom.epc,
          }).then((res) => {
            this.$message({
              message: "添加成功",
              type: "success",
            });
            this.$router.go(-1);
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
