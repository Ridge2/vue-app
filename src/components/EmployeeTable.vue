<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>
    <table v-else>
      <tbody>
        <!-- This is an array -->
        <tr v-for="employee in employees" :key="employee.id">
          <!-- Code for editing -->
          <!-- If else editing technique in Vue-->
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{employee.name}}</td>
          <!-- -------------------------------- -->  
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{employee.email}}</td>
          <!-- -------------------------------- -->  
          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(employee.id)">Edit</button>
            <button @click="$emit('delete:employee', employee.id)">Delete</button>
          </td>
          <!-- -------------------------------- -->  
        </tr>
      </tbody>
    </table>
  </div>
</template>
//
<script>
export default {
  name: 'employee-table',
  props: {
    employees: Array,
  },
  data() {
    return {
      // When using v-model, always initial the data component
      //
      editing: null,
    }
  },
  methods: {
    // Used for when in editing mode.
    //
    editMode(id) {
      this.editing = id
    },
    editEmployee(employee) {
      if (employee.name === '' || employee.email === '') return
      this.$emit('edit:employee', employee.id, employee) // Send this information back to the function
      this.editing = null
    },
    // I have to find out why this wasn't doing what I want.
    /*
    editMode(employee) {
      this.cachedEmployee = Object.assign({}, employee)      
      this.editing = employee.id
    },
    */
    cancelEdit(employee) {
      Object.assign(employee, this.cachedEmployee)
      this.editing = null;
    },
  }
} 
</script>

<style scoped>
  button {
    margin: 0 0.5rem 0 0;
  }
</style>