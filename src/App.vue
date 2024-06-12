<template>

  <div class="small-container">
    <h1>Employee Project</h1>
    <employee-form @add:employee="addEmployee"></employee-form>
    <!---we can use v-bind to bind the component or tell the component where it should look int for what--->
    <!--- :attribute = "objectname"--->
    <!--attribute can be a different nane than object--->
    <employee-table :getEmployess='employees'
                    @delete:employee="deleteEmployee"
                    @save:employee="saveEmployee">
    </employee-table>
    <!---this @eventname is used to listen to the events from the child component followed by the function name that needs to be performed by the parent class--->
  </div>
</template>

<script>


import EmployeeTable from "@/components/EmployeeTable";
import EmployeeForm from "@/components/EmployeeForm";

export default {
  name: 'App',
  components: {
    EmployeeForm,
    EmployeeTable

  },
  data() {
    return {
      employees: []
    }
  },
  methods: {
    async addEmployee(employee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users',
            {
              method: 'POST',
              //to convert the employee object to Json format
              body: JSON.stringify(employee),
              headers: {'Content-type': 'application/json; charset=UTF-8'}
            });
        const data = await response.json();
        this.employees = [...this.employees, data]
      } catch (e) {
        console.log(e);
      }
      //this will help to add the even details into the existing array object if it is array [...existing arrayname,objectname], this will helps to save the existing data
      //without getting deleted and add the data at last
      // const id = this.employees.length>0? (this.employees.length-1)+2 : 1;
      // const newEmployee = {...employee,id}
      // this.employees=[...this.employees,newEmployee]

    },
    async deleteEmployee(id) {
      try {
        await fetch('https://jsonplaceholder.typicode.com/users/' + id, {
          method: 'DELETE'
        })
        this.employees = this.employees.filter((employee) => {
          return employee.id != id;
        })
      } catch (e) {
        console.log(e);
      }
    },
    async saveEmployee(id, updatedEmployee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users/' + id, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: {'Content-type': 'application/json; charset=UTF-8'}
        });
        const data = await response.json();
        this.employees.map((employee) => {
          return employee.id == id ? data : employee;
        })
      } catch (e) {
        console.log(e);
      }
    },
    //This async is to handle the api request, if there is a delay in the response then we can you this async
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {methoid: 'GET'});
        //response will be in text so to convert it into json we are using .json method
        //even for converting the await response it needs to be awaiting
        const data = await response.json();
        this.employees = data;

      } catch (e) {
        console.log(e);

      }

    }
  },
  //mounted is a function property, sowhenever the particualr this component is loaded mounted will bw the first function to be called
  mounted() {
    this.getEmployees();

  }
}
</script>

<style>
.small-container {
  max-width: 780px
}
</style>
