<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <employee-form @add:employee="addEmployee" />
    <employee-table
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from "./components/EmployeeTable";
import EmployeeForm from "./components/EmployeeForm";

export default {
  name: "app",
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: []
    };
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users"
        );
        const data = await response.json();
        this.employees = data;
      } catch (error) {
        console.error(error);
      }
    },
    //post
    async addEmployee(employee) {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users",
          {
            method: "POST",
            body: JSON.stringify(employee),
            headers: { "Content-type": "application/json; charset=UTF-8" }
          }
        );

        const data = await response.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        console.error(error);
      }
    },

    //add id to new employee
    // const lastId =
    //   this.employees.length > 0
    //     ? this.employees[this.employees.length - 1].id
    //     : 0;
    // const id = lastId + 1;
    // //展开语法(Spread syntax), 可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开；还可以在构造字面量对象时, 将对象表达式按key-value的方式展开。(译者注: 字面量一般指 [1, 2, 3] 或者 {name: "mdn"} 这种简洁的构造方式)
    // //https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_syntax
    // const newEmployee = { ...employee, id };
    // this.employees = [...this.employees, newEmployee];
    // },

    async deleteEmployee(id) {
      try {
        await fetch(
          `https://jsonplaceholder.typicode.com/users/${id}`,
          {
            method: "DELETE"
          },
          (this.employees = this.employees.filter(
            employee => employee.id !== id
          ))
        );
      } catch (error) {
        console.error(error);
      }
    },

    // map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。
    //map through the employees array, and update the correct employee.
    async editEmployee(id, updateEmployee) {
      try {
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/users/${id}`,
          {
            method: "PUT",
            headers: { "Content-type": "application/json; charset=UTF-8" },
            body: JSON.stringify(updateEmployee)
          }
        );
        const data = await response.json();
        this.employees = this.employees.map(employee => {
          return employee.id === id ? data : employee;
        });
      } catch (error) {
        console.error(error);
      }
    }
  },
  mounted() {
    this.getEmployees();
  }
};
</script>

<style lang="scss">
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
