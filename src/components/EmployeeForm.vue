<template>
    <div id="employee-form">
        <form v-on:submit.prevent="handleSubmit">
            <label>Employee Name</label>
            <input  type="text"
                    ref="first"
                    v-bind:class = "{'has-error': submitting & invalidName}"
                    v-model = "employee.name"
                    v-on:focus = 'clearStatus'
                    v-on:keypress = 'clearStatus'
                    >
            <label>Employee Email</label>
            <input  type="email"
                    v-bind:class = "{'has-error': submitting & invalidEmail}"
                    v-model="employee.email"
                    v-on:focus = 'clearStatus'
                    
                    >

            <p v-if = "error & submitting" class="error-message">
                ❗ Please fill out all required fields
            </p>
            <p v-if = "success" class="success-message">
                ✅ Employee successfully added
            </p>
            <button>Add Employee</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'employee-form',
    data() {
        return {
            submitting: false,
            error: false,
            success:false,
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

            if(this.invalidName || this.invalidEmail) {
                this.error = true
                return
            }

            this.$emit('add:employee', this.employee)
            this.$refs.first.focus()
            this.employee = {
                name: '',
                email: ''
            }
            this.error = false,
            this.success = true,
            this.submitting = false
        },
        clearStatus() {
            this.success = false,
            this.error = false
        }
    },
    computed: {
        invalidName() {
            return this.employee.name === ''
        },
        invalidEmail() {
            return this.employee.email === ''
        }
    }
}
</script>

<style scope>
    form {
        margin-bottom: 2rem;
    }

    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: red;
    }

    .success-message {
        color: green;
    }
</style>