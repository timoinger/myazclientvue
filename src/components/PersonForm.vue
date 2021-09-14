<template>
    <div id="employee-form">
        <form @submit.prevent="handleSubmit" >
            <p v-if="error && submitting" class="error-message">
                ❗Please fill out all required fields
            </p>
            <p v-if="success" class="success-message">
                ✅ Employee successfully added
            </p>

            <label>Enter</label>
            <input v-model="name" type="text"
                   :class="{ 'has-error': submitting && invalidName }"
                   @focus="clearStatus"
                   @keypress="clearStatus" />

            <button>Add</button>
        </form>
    </div>
</template>

<script lang="ts">
export default {
    name: "PersonForm",
    data() {
        return {
            submitting: false,
            error: false,
            success: false,
            name: '',
        }
    },
    methods: {
        handleSubmit() {
            this.submitting = true
            this.clearStatus();

            if (this.invalidName) {
                this.error = true
                return
            }
            this.$emit('add:person', this.name)

            this.error = false
            this.success = true
            this.submitting = false
        },
        clearStatus() {
            this.success = false
            this.error = false
        },
    },
    computed: {
        invalidName() {
            return this.name === ''
        },
    },
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
