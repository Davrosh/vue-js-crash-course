<template>
  <div id="employee-form">
    <form v-on:submit.prevent="handleSubmit">
      <label for="name">Name:</label>
      <input
      type="text"
      name="name"
      ref="first"
      :class="{ 'has-error': submitting && invalidName, 'is-success': success }"
      v-model="employee.name"
      @focus="clearStatus"
      @keypress="clearStatus"
      />
      <label
        for="email"
      >Email:</label>
      <input
        type="email"
        name="email"
        :class="{ 'has-error': submitting && invalidEmail, 'is-success': success }"
        v-model="employee.email"
        @focus="clearStatus"
      />
      <p v-if="error" class="error-message">Please fill out all required fields</p>
      <p v-if="success" class="success-message">Employee successfully added</p>
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
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmail) {
        // this.submitting = false;
        this.error = true;
        return;
      }

      this.$emit("add:employee", this.employee);
      this.$refs.first.focus();
      this.employee = {
        name: "",
        email: ""
      };

      this.error = false;
      this.submitting = false;
      this.success = true;
    },
    clearStatus() {
      this.success = false;
      this.error = false;
    }
  },
  computed: {
    invalidName() {
      return this.employee.name === "";
    },

    invalidEmail() {
      return this.employee.email === "";
    }
  }
};
</script>

<style scoped>
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