<template>
  <el-form
    :label-position="labelPosition"
    :label-width="labelWidth"
    :model="model"
    :rules="rules"
    ref="form"
  >
    <el-form-item label="First name" prop="firstName">
      <el-input v-model="model.firstName" placeholder="First name"></el-input>
    </el-form-item>
    <el-form-item label="Last Name" prop="lastName">
      <el-input v-model="model.lastName" placeholder="Last name"></el-input>
    </el-form-item>
    <el-form-item label="Company" prop="company">
      <el-input v-model="model.company" placeholder="Company"></el-input>
    </el-form-item>
    <el-form-item label="Job Title" prop="title">
      <el-input v-model="model.title" placeholder="Title"></el-input>
    </el-form-item>
    <el-form-item label="Email" prop="email">
      <el-input v-model="model.email" placeholder="Email"></el-input>
    </el-form-item>
  </el-form>
</template>

<script>
export default {
  data() {
    return {
      labelPosition: "left",
      labelWidth: "100px",
      model: {
        firstName: "",
        lastName: "",
        company: "",
        title: "",
        email: "",
      },
      rules: {
        firstName: [
          {
            required: true,
            message: "First name is required",
            trigger: "blur",
          },
        ],
        lastName: [
          {
            required: true,
            message: "Last name is required",
            trigger: "blur",
          },
        ],
        company: [
          {
            required: true,
            message: "Company name is required",
            trigger: "blur",
          },
        ],
        title: [
          {
            required: true,
            message: "Job Title is required",
            trigger: "blur",
          },
        ],
        email: [
          {
            required: true,
            message: "Email is required",
            trigger: "blur",
          },
          {
            type: "email",
            message: "Invalid email",
            trigger: "change",
          },
        ],
      },
    };
  },
  methods: {
    validate() {
      console.log("FirstStep:validate");
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
