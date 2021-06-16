<template>
  <div>
    <div>{{ sumPrice }}</div>
    <el-form ref="form" :model="form" :rules="rules">
      <el-form-item label="房子所占面积">
        <el-input
          v-model="form.house_size"
          type="'number'"
          placeholder="请输入房子所占面积"
          controls-position="right"
        ></el-input>
      </el-form-item>
      <el-form-item label="房子总价格">
        <el-input
          v-model="form.house_price"
          type="'number'"
          placeholder="请输入房产总价格"
          controls-position="right"
        ></el-input>
      </el-form-item>
      <el-form-item label="需付首付比例" prop="first_rate">
        <el-input
          v-model.number="form.first_rate"
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
      <el-form-item label="贷款方式">
        <el-radio-group v-model="form.load_method">
          <el-radio label="business">商业贷款</el-radio>
          <el-radio label="fund">组合贷款（公积金贷款 + 商业贷款）</el-radio>
        </el-radio-group>
      </el-form-item>
    </el-form>
    <el-form ref="fundForm" :model="fundForm" :rules="fundRules" v-if="form.load_method === 'fund'">
      <el-form-item label="公积金贷款金额">
        <el-input
          v-model="form.fund_price"
          type="'number'"
          placeholder="请输入公积金贷款金额"
        ></el-input>
      </el-form-item>
      <el-form-item label="商业贷款金额">
        <el-input
          v-model="form.business_price"
          type="'number'"
          placeholder="请输入商业贷款金额"
        ></el-input>
      </el-form-item>
      <el-form-item label="商业贷款利率">
        <el-input
          v-model="form.business_rate"
          type="'number'"
          placeholder="请输入商业贷款利率"
        ></el-input>
      </el-form-item>
    </el-form>
    <el-button @click="handleHouseFirstPrice">计算首付</el-button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import {
  ElForm,
  ElFormItem,
  ElInput,
  ElRadio,
  ElRadioGroup,
  ElButton,
} from "element-plus";

export default defineComponent({
  name: "App",
  components: {
    ElForm,
    ElFormItem,
    ElInput,
    ElRadio,
    ElRadioGroup,
    ElButton,
  },
  data() {
    const validateFirstRate = (
      rule: any,
      value: string,
      callback: Function
    ) => {
      const numValue = parseFloat(value);
      if (!Number.isInteger(value)) {
        callback(new Error("请输入数字值"));
      } else if (numValue > 1 || numValue <= 0) {
        callback(new Error("请输入数字值满足 ===> 大于0 小于等于1"));
      } else {
        callback();
      }
    };
    return {
      form: {
        house_price: undefined,
        mediation_price: undefined,
        first_rate: undefined,
        deed_tax: undefined,
        house_size: undefined,
      },
      fundForm: {
        fund_price: undefined,
        business_price: undefined,
        business_rate: undefined,
        fund_safe: 2000
      },
      rules: {
        first_rate: [{ validator: validateFirstRate, trigger: "blur" }],
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
      const deedTaxMoney = house_price * deed_tax;
      return firstMoney + mediationMoney + deedTaxMoney;
    },
  },
  methods: {
    handleHouseFirstPrice() {
      console.log(this.$data.form);
    },
  },
  mounted() {},
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
