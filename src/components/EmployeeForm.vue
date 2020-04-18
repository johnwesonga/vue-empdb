<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label>Employee name</label>
      <input
        ref="first"
        v-model="employee.name"
        :class="{ 'has-error': submitting && invalidName }"
        type="text"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Employee Email</label>
      <input
        v-model="employee.email"
        :class="{ 'has-error': submitting && invalidEmail }"
        type="text"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Employee Role</label>
      <input
        v-model="employee.role"
        :class="{ 'has-error': submitting && invalidRole }"
        type="text"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button>Add Employee</button>
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
        email: "",
        role: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      //console.log("testing handleSubmit");
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmail || this.invalidRole) {
        this.error = true;
        return;
      }
      this.$emit("add:employee", this.employee);
      this.$refs.first.focus();
      this.employee = {
        name: "",
        email: "",
        role: ""
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
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
    },

    invalidRole() {
      return this.employee.role === "";
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
