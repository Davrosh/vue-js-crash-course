<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>
    <table v-else>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>

      <tbody v-for="employee in employees" v-bind:key="employee.id">
        <tr v-show="editing===employee.id">
          <td hidden>
            <form v-bind:id="employee.id" v-on:submit.prevent="editEmployee(employee)"></form>
          </td>
          <td>
            <input type="text" v-model="employee.name" v-bind:form="employee.id" />
          </td>
          <td>
            <input type="email" v-model="employee.email" v-bind:form="employee.id" />
          </td>
          <td>
            <button v-bind:form="employee.id">Save</button>
            <button
              class="muted-button"
              v-on:click="cancelEdit(employee)"
              v-bind:form="employee.id"
            >Cancel</button>
          </td>
        </tr>

        <tr v-show="editing!==employee.id">
          <td>{{employee.name}}</td>
          <td>{{employee.email}}</td>
          <td>
            <button v-on:click="editMode(employee)">Edit</button>
            <button v-on:click="$emit('delete:employee',
            employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    employees: Array
  },
  data() {
    return {
      editing: null,
      cachedEmployee: null
    };
  },
  methods: {
    editMode(employee) {
      this.cachedEmployee = Object.assign({}, employee);
      this.editing = employee.id;
    },
    editEmployee(employee) {
      if (employee.name === "" || employee.email === "") {
        return;
      }
      this.$emit("edit:employee", employee.id, employee);
      this.editing = null;
    },
    cancelEdit(employee) {
      this.editing = null;
      Object.assign(employee, this.cachedEmployee);
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>