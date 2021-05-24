<template>
    <div id="employee-table">
        <p v-if="employees.length < 1" class="empty-table">No employees</p>
        <table v-else>
            <thead>
                 <tr>
                    <th>Employee name</th>
                    <th>Employee email</th>
                    <th>Action</th>
                </tr>
            </thead>

            <tbody>
                <tr v-for="employee in employees" :key="employee.id"> <!--The same as using JSX to map through elements in React. It needs a key for each item-->

                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.name" />
                    </td>
                    <td v-else>{{employee.name}}</td> <!--How do I destructure it in Vue?-->

                    
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.email" />
                    </td>
                    <td v-else>{{employee.email}}</td> 


                   <td v-if="editing === employee.id">
                        <button @click="editEmployee(employee)">Save</button>
                        <button class="muted-button" @click="editing = null">Cancel</button>
                    </td>
                    <td v-else>
                        <button @click="editMode(employee.id)">Edit</button>
                        <button @click="$emit('delete:employee', employee.id)">Delete</button>
                    </td>
                </tr>
                    

            </tbody>

        </table>
    </div>
</template>

<script>
export default {
    name: 'employee-table',
    props: {
        employees: [], // Come back to this. If this doesn't work, change the [] to Array
    },
    data() {
        return {
            editing: null,
        }
    },
    methods: {
        editMode(id) {
            this.editing = id; // Edit doesn't work :(
        },

        editEmployee(updatedEmployee) {
            if (updatedEmployee.name === '' || updatedEmployee.email === '') return
            this.$emit('edit:employee', updatedEmployee.id, updatedEmployee)
            this.editing = null
        },

        
    }
}
</script>

<style scoped>
    button {
        margin: 0 0.5rem 0 0;
    }
</style>