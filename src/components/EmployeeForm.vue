<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label>Employee name</label>
      <!-- this line makes the cursor refocus on the first input line.-->
      <input ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="employee.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Employee Email</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="employee.email"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">
        ❗Please fill out all required fields
      </p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
  export default {
    name: 'employee-form',
    data() {
      return {
        // These are called states
        submitting: false,
        error: false,
        success: false,
        //
        employee: {
          name: '',
          email: '',
        },
      }
    },
    methods: {
        handleSubmit() {
            this.submitting = true
            this.clearStatus()

            if(this.invalidName || this.invalidEmail) {
                this.error = true
                return
            }
            this.error = false
            this.success = true
            this.submitting = false
            // this adds the employee object to the list.
            this.$emit('add:employee', this.employee)
            this.$refs.first.focus()
        },
        // New function always are to the bottom.
        clearStatus() {
            this.success = false
            this.error = false
        }
    },
    //These functions are automatically computed when something changes.
    computed: {
        invalidName() {
            return this.employee.name === ''
        },
        invalidEmail() {
            return this.employee.email === ''
        },
    },
  }
</script>

<style scoped>
  form {
    margin-bottom: 2rem;
  }

  [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>