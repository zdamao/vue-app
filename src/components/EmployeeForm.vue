
<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label for="name">Name</label>
      <input
        type="text"
        v-model="employee.name"
        :class="{'has-error': submitting && invalidName}"
        @keyup="clearStatus"
        @focus="clearStatus"
        ref="first"
      />
      <label for="email">Email</label>
      <input
        type="text"
        v-model="employee.email"
        :class="{'has-error': submitting && invalidEmail}"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button>Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: ""
      }
    };
  },
  computed: {
    invalidName() {
      return this.employee.name === "";
    },
    invalidEmail() {
      return this.employee.email === "";
    }
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }

      this.$emit("add:employee", this.employee);
      this.employee = {
        name: "",
        email: ""
      };
      this.$refs.first.focus();
      this.submitting = false;
      this.error = false;
      this.success = true;
    },
    clearStatus() {
      this.error = false;
      this.success = false;
    }
  }
};
</script>

<style lang='scss' scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
