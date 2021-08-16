<template>
  <div id="app">
    <pre v-html="prettyJSON"></pre>
    <form-wizard
      id="form-wizard-1"
      @on-complete="onComplete"
      @on-loading="setLoading"
      @on-validate="handleValidation"
      @on-error="handleErrorMessage"
      ref="wizard"
      :start-index.sync="activeTabIndex"
      shape="circle"
      color="#20a0ff"
      error-color="#ff4949"
    >
      <tab-content
        title="Personal details"
        icon="ti-user"
        :before-change="() => validate('firstStep')"
      >
        <first-step ref="firstStep" @on-validate="onStepValidate"></first-step>
      </tab-content>
      <tab-content
        title="Verify email"
        icon="ti-unlock"
        :before-change="() => validate('secondStep')"
      >
        <second-step
          ref="secondStep"
          @on-validate="onStepValidate"
        ></second-step
        ><el-button @click="forceClearError">Try to clear the error</el-button>
      </tab-content>
      <tab-content title="Complete" icon="ti-check">
        Your data
        <pre v-html="prettyJSON"></pre>
      </tab-content>

      <div v-if="errorMsg">
        <el-alert title="error" type="error" :description="errorMsg" show-icon>
        </el-alert>
        <!-- <span class="error">{{ errorMsg }}</span> -->
      </div>
    </form-wizard>
  </div>
</template>

<script>
import FirstStep from "./components/FirstStep.vue";
import SecondStep from "./components/SecondStep.vue";
import { Loading } from "element-ui";
import prettyJSON from "./prettyJson.js";
export default {
  name: "app",
  components: {
    FirstStep,
    SecondStep,
  },
  data() {
    return {
      finalModel: {},
      activeTabIndex: 0,
      errorMsg: null,
      loadingInstance: null,
    };
  },
  computed: {
    prettyJSON() {
      return prettyJSON(this.finalModel);
    },
  },
  methods: {
    onComplete() {
      alert("Yay. Done!");
    },
    forceClearError() {
      console.log("forceClearError");
      this.$refs.wizard.tabs[this.activeTabIndex].validationError = null;
    },
    validate(ref) {
      console.log("validate", ref);
      return this.$refs[ref].validate().then((valid) => {
        if (!valid) {
          return valid;
        }
        let response = null;
        switch (ref) {
          case "firstStep":
            response = this.sendOtpRequest();
            break;
          case "secondStep":
            response = this.validateOtpRequest();
            break;

          default:
            response = new Promise.resolve(true);
            break;
        }
        return response;
      });
    },
    onStepValidate(validated, model) {
      console.log("onStepValidate", validated);
      if (validated) {
        this.finalModel = { ...this.finalModel, ...model };
      }
    },
    setLoading: function (value) {
      console.log(
        "setLoading",
        value,
        this.loadingInstance,
        new Date().toISOString()
      );
      // if (value) {
      //   this.loadingInstance = Loading.service({ target: "#form-wizard-1" });
      // } else {
      //   if (this.loadingInstance) {
      //     this.loadingInstance.close();
      //   }
      // }
    },
    handleValidation: function (isValid, tabIndex) {
      console.log("handleValidation");
      console.log("Tab: " + tabIndex + " valid: " + isValid);
    },
    handleErrorMessage: function (errorMsg) {
      console.log("handleErrorMessage");
      this.errorMsg = errorMsg;
    },
    sendOtpRequest: function () {
      const self = this;
      console.log("sendOtpRequest....");
      console.log(JSON.stringify(this.finalModel));
      return new Promise((resolve, reject) => {
        const loadingInstance = Loading.service({ target: "#form-wizard-1" });
        setTimeout(() => {
          // console.log("reject: sendOtpRequest....");
          // reject("Could not send OTP. Please try again in some time");
          resolve(true);
          if (loadingInstance) {
            loadingInstance.close();
          }
        }, 1000);
      });
    },
    validateOtpRequest: function () {
      console.log("validateOtpRequest....");
      console.log(JSON.stringify(this.finalModel));
      return new Promise((resolve, reject) => {
        const loadingInstance = Loading.service({ target: "#form-wizard-1" });
        setTimeout(() => {
          // console.log("reject: validateOtpRequest....");
          // reject("invalid OTP. Please try again");
          if (loadingInstance) {
            loadingInstance.close();
          }
        }, 1000);
      });
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  font-family: Menlo, Monaco, "Courier New", monospace;
  font-weight: normal;
  font-size: 14px;
  line-height: 16px;
  margin: 0;
}

pre {
  overflow: auto;
}
pre .string {
  color: #885800;
}
pre .number {
  color: blue;
}
pre .boolean {
  color: magenta;
}
pre .null {
  color: red;
}
pre .key {
  color: green;
}

.panel {
  margin-bottom: 20px;
  background-color: #fff;
  border: 1px solid transparent;
  border-radius: 4px;
  -webkit-box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
  border-color: #ddd;
}

.panel-heading {
  color: #333;
  background-color: #f5f5f5;
  border-color: #ddd;

  padding: 10px 15px;
  border-bottom: 1px solid transparent;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}

.panel-body {
  padding: 15px;
}
</style>
