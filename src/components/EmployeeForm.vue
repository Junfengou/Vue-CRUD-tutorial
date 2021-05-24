<template >
    <div id="employee-form">
        <form @submit.prevent="handleSubmit"> <!--same as [v-on:submit] and you can chain the e.preventDefault() on here-->
            <label>Employee name</label>
            <input 
                v-model="employee.name" 
                type="text" 
                :class="{ 'has-error': submitting && invalidName }" 
                @focus="clearStatus"
                @keypress="clearStatus"
                ref="first"
            />
            <!--[@focus] check to see whether the mouse is targeting the input box-->
            <!--What is [:class=]-->

            <label>Employee email</label>
            <input 
                v-model="employee.email" 
                type="text" 
                :class="{ 'has-error': submitting && invalidEmail }" 
                @focus="clearStatus"
            /> <!--[v-model] is Vue's onChange()-->

            <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
            <p v-if="success" class="success-message">✅ Employee successfully added</p>
            <button>Add employee</button>
        </form>
    </div>
</template>

<script>
export default {
    
    name: "employee-form",
    data() { // state
        return {
            submitting: false,
            error: false,
            success: false,
            employee: { 
                name: '',
                email: ''
            }
        }
    },
    methods: {
        handleSubmit() {
            
            this.submitting = true
            this.clearStatus()

            // checking whether the input fields are empty on submit 
            if(this.invalidName || this.invalidEmail) {
                this.error = true
                return
            }
            
            /*  Vue doesn't callback to serve state up to parent component
                [this.$emit] will serve the state up to parent component
                [add:employee] is the event we're serving up
            */
            this.$emit('add:employee', this.employee) 
            // Automatically focus on the first input box after submitting the data for easier access
            this.$refs.first.focus() 
            // console.log(`test ${this.employee.name} - ${this.employee.email}`);

            this.employee = {
                name: '',
                email: '',
            }
            this.error = false
            this.success = true
            this.submitting = false
        },

        clearStatus() {
            this.success = false
            this.error = false
        }
    }, 
    computed: { 
        // Form validation
        // This field check for form logic like null values?
        invalidName() {
            return this.employee.name === ""
        },

        invalidEmail() {
            return this.employee.email === ""
        },
    }
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