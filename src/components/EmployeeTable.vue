<template>
  <p v-if="getEmployess.length<1"> No employees</p>
  <table v-else>
    <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>Years of experience</th>
      <th>DOJ</th>
      <th>Action</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for = "employee in getEmployess" :key="employee.id" >

      <td v-if="editing===employee.id">
        <input type="text" v-model="employee.name">
      </td>
      <td v-else>{{employee.name}}</td>

      <td v-if="editing===employee.id">
        <input type="number" v-model="employee.Age">
      </td>
      <td v-else>{{employee.Age}}</td>

      <td  v-if="editing===employee.id">
        <input type="number" v-model="employee.Yearsofexperience">
      </td>
      <td v-else>{{employee.Yearsofexperience}}</td>

      <td>{{employee.DOJ}}</td>
      <td v-if="editing ===employee.id">
        <button class = "button-display" @click = "saveEmployee(employee)">Save</button>
        <button class = "muted-button" @click="cancelEmployee(employee)">Cancel</button>
      </td>
      <td v-else>
        <button class = "button-display" @click="editMode(employee)">Edit</button>
        <button class = "button-display" @click="$emit('delete:employee',employee.id)">Delete</button>
      </td>
    </tr>
    </tbody>
  </table>
</template>
<script>
export default {
  name : 'employee-table',
  props:{
    getEmployess: Array
  },
  data(){
    return{
      editing: null,
      cachedEmployee:null
    }
  },
  methods:{
    editMode(employee){
      this.editing = employee.id;
      this.cachedEmployee=Object.assign({},employee);
    },
    saveEmployee(employee){
      if(employee.name ===''){
        return;
      }
      //can send multiple parameter inside the emit event
      this.$emit('save:employee',employee.id, employee);
      this.editing = null;
    },
    cancelEmployee(employee){
      this.editing = null;
      Object.assign(employee, this.cachedEmployee)

    }
  }
}
</script>
<style scoped>
.button-display{
  margin: 0 0.5rem 0 0;
  background: #1cb864;
  border-color: #1cb864;
}

</style>