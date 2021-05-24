<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form @add:employee="addEmployee" />
    <employee-table 
      :employees="employees" 
      @delete:employee="deleteEmployee" 
      @edit:employee="editEmployee"
      /> <!--short hand syntax to pass state as props down to child--->
  </div>
</template>

<script>
import EmployeeForm from './components/EmployeeForm.vue'
import EmployeeTable from './components/EmployeeTable.vue'


export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() { // state
    return {
      employees: []
    }
  },
  methods: {
    async addEmployee(employee) { 
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
        })
        const newEmployee = await response.json()
        this.employees = [...this.employees, newEmployee]
      } 
      catch(e) {
        console.error(e);
      }
    },

    async deleteEmployee(id) {
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: "DELETE"
        });
        this.employees = this.employees.filter((employee) => employee.id !== id) // match the condition? then the item can stay
      }
      catch(e) {
        console.error(e);
      }
    }, 

    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-type': 'application/json; charset=UTF-8' },
      })
        const data = await response.json()
        this.employees = this.employees.map(employee => employee.id === id ? data : employee)
      }
      catch(e) {
        console.error(e);
      }
    },

    async getEmployees() {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users")
        const data = await response.json()
        this.employees = data
      }
      catch(e) {
        console.error(e);
      }
    },


  },

  mounted() {
    // Lifecycle stuff
    // fetch information from url on render
    this.getEmployees()
  }
}
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>
