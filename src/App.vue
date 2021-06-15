<template>
  <div>
    <div>{{ sumPrice }}</div>
    <el-form ref="form" :model="form">
      <el-form-item label="房子价格">
        <el-input
          v-model="form.house_price"
          type="'number'"
          placeholder="请输入房产总资产"
        ></el-input>
      </el-form-item>
      <el-form-item label="需付首付比例">
        <el-input
          v-model="form.first_rate"
          type="'number'"
          placeholder="请输入需付首付比例"
        ></el-input>
      </el-form-item>
      <el-form-item label="中介收费占比">
        <el-input
          v-model="form.mediation_price"
          type="'number'"
          placeholder="请输入中介收费占比"
        ></el-input>
      </el-form-item>
      <el-form-item label="契税">
        <el-input
          v-model="form.deed_tax"
          type="'number'"
          placeholder="请输入契税占比"
        ></el-input>
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { ElForm, ElFormItem, ElInput } from "element-plus";

export default defineComponent({
  name: "App",
  components: {
    ElForm,
    ElFormItem,
    ElInput,
  },
  data() {
    return {
      form: {
        house_price: undefined,
        mediation_price: undefined,
        first_rate: undefined,
        deed_tax: undefined,
      },
    };
  },
  computed: {
    sumPrice() {
      const house_price: number = this.$data.form.house_price || 0;
      const mediation_price: number = this.$data.form.mediation_price || 0;
      const first_rate: number = this.$data.form.first_rate || 0;
      const deed_tax: number = this.$data.form.deed_tax || 0;
      const firstMoney = house_price * first_rate;
      const mediationMoney = house_price * mediation_price;
      const deedTaxMoney = house_price * deed_tax
      return firstMoney + mediationMoney + deedTaxMoney;
    },
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
