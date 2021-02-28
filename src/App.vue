<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <employee-form @add:employee="addEmployee" />
    <table-header :employees="employees" />

    <!-- This section connects the functions with the function code at the bottom-->
    <employee-table :employees="employees" 
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"/>
  </div>
</template>

<script>
  import TableHeader from '@/components/TableHeader.vue'
  import EmployeeTable from '@/components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'

  export default {
    name: 'app',
    components: {
      TableHeader,
      EmployeeTable,
      EmployeeForm,
    },
    data() {
      // ...
      return {
        employees: [],
      }
    },
    methods: {
      async getEmployees() {
        try {
          const response = await
          fetch('https://jsonplaceholder.typicode.com/users')
          const data = await response.json()
          this.employees = data
        } catch (error) {
          console.error(error)
        }
      },
      async addEmployee(employee) {
        try {
          const response = await
          fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
          const data = await response.json()
          this.employee = [...this.employees, data]
        } catch (error) {
          console.error(error)
        }
      },
      async deleteEmployee(id)  {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: "DELETE"
          });
          this.employees = this.employees.filter(employee =>
          employee.id !== id);
        } catch (error) {
          console.error(error);
        }
      },
      async editEmployee(id, updateEmployee) {
        try {
          const response = await
          fetch(`https://jsonplaceholder.typicode.com/users/${id}`, { 
            method: 'PUT',
            body: JSON.stringify(updateEmployee),
            headers: {'Content-type':'application/json; charset=UTF-8'},
          })
          const data = await response.json()
          this.employees = this.employees.map(employee =>(employee.id === id ? data : employee))
        } catch (error) {
          console.error(error)
        }
      },
    },
    mounted() {
      this.getEmployees()
    },
  }
</script>