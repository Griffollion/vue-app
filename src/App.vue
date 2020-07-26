<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <employee-form v-on:add:employee="addEmployee" />
    <employee-table
      v-bind:employees="employees"
      v-on:delete:employee="deleteEmployee"
      v-on:edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from "@/components/EmployeeTable.vue";
import EmployeeForm from "@/components/EmployeeForm.vue";

export default {
  name: "app",
  components: {
    EmployeeTable,
    EmployeeForm,
  },
  data() {
    return {
      employees: [],
    };
  },
  mounted() {
    this.getEmployees();
  },
  methods: {
    async getEmployees() {
      try {
        const res = await fetch("http://localhost:3002/users");
        const data = await res.json();
        this.employees = data;
      } catch (error) {
        console.log(error);
      }
    },

    async addEmployee(employee) {
      try {
        const res = await fetch("http://localhost:3002/users", {
          method: "POST",
          body: JSON.stringify(employee),
          headers: { "Content-type": "application/json; charset=UTF-8" },
        });
        const data = await res.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        console.log(error);
      }
    },
    async deleteEmployee(id) {
      try {
        const res = await fetch(`http://localhost:3002/users/${id}`, {
          method: "DELETE"
        })

        this.employees = this.employees.filter(employee => employee.id !== id)
      } catch (error) {
        console.log(error)
      }
    },
    async editEmployee(id, updatedEmployee) {
      console.log(updatedEmployee);
      try {
        const res = await fetch(`http://localhost:3002/users/${id}`, {
          method: "PUT",
          body: JSON.stringify(updatedEmployee),
          headers: { "Content-type": "application/json; charset=UTF-8" },
        });

        const data = await res.json();
        this.employees = this.employees.map((employee) =>
          employee.id === id ? data : employee
        );
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
button {
  background-color: red;
  border: 1px solid rgb(133, 31, 31);
}

.small-container {
  max-width: 680px;
}
</style>
