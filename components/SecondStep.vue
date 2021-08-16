<template>
  <el-form
    :label-position="labelPosition"
    :label-width="labelWidth"
    :model="model"
    :rules="rules"
    ref="form"
  >
    <el-form-item label="OTP" prop="otp">
      <el-input
        type="tel"
        :maxlength="maxlength"
        v-model="model.otp"
        placeholder="6 digit OTP"
      ></el-input>
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  data() {
    return {
      labelPosition: "left",
      labelWidth: "100px",
      maxlength: 6,
      model: {
        otp: null,
      },
      rules: {
        otp: [
          {
            required: true,
            message: "OTP is required",
            trigger: "blur",
          },
          {
            type: "regexp",
            range: "^[0-9]*$",
            message: "Should be a 6 digit number",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
    validate() {
      console.log("SecondStep:validate");
      return new Promise((resolve, reject) => {
        this.$refs.form.validate((valid) => {
          this.$emit("on-validate", valid, this.model);
          resolve(valid);
        });
      });
    },
  },
};
</script>

<style>
</style>
