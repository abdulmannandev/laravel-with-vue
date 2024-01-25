<template>
    <div id="todo-form">
        <form @submit.prevent="handleSubmit">
            <div class="form-item">
                <label>Task Name: </label>
                <input 
                    type="text"
                    name="title"
                    :class="{ 'has-error': submitting && isTitleValid }"
                    v-model="task.title"
                    @focus="clearStatus"
                    @keypress="clearStatus" 
                    @change="onChange"
                />
            </div>
            <div class="form-item">
                <label>Periorty: </label>
                <select
                    name="periorty"
                    :class="{ 'has-error': submitting && isPeriortyValid }"
                    @change="onChange"
                >
                    <option :selected="true">Please select one</option>
                    <option v-for="option in task.periorties" v-bind:selected="option.value === task.periorty" v-bind:key="option.value" v-bind:value="option.value" >{{ option.name }}</option>
                </select>
            </div>
            <div class="form-item">
                <label>Task Description: </label>
                <textarea 
                    type="text"
                    name="description"
                    v-model="task.description"
                    @change="onChange"
                    :class="{ 'has-error': submitting && isDescriptionValid }"
                    @focus="clearStatus"
                    @keypress="clearStatus" 
                />
            </div>
            <p v-if="error && submitting" class="error-message">
                ❗Please fill out all required fields
            </p>
            <p v-if="success && message !== ''" class="success-message">
                ✅ {{message}}
            </p>
            <button>Add Task</button>
        </form>
    </div>
</template>

<script>
    export default {
        name: 'todo-form',
        props: {
            message: String
        },
        data() {
            return{
                submitting: false,
                error: false,
                success: false,
                task: {
                    title: '',
                    periorty: '',
                    description: '',
                    periorties: [
                        {
                            name: 'High',
                            value: 'HIGH',
                        },
                        {
                            name: 'Medium',
                            value: 'MEDIUM',
                        },
                        {
                            name: 'Low',
                            value: 'LOW',
                        }
                    ]
                },
            }
        },
        methods: {
            handleSubmit() {
                this.submitting = true
                this.clearStatus()

                if(this.isTitleValid || this.isDescriptionValid || this.isPeriortyValid){
                    this.error = true
                    return
                }

                this.$emit('add:task', this.task)
                this.task = {
                    ...this.task,
                    title: '',
                    periorty: '',
                    description: '',
                }
                this.error = false
                this.success = true
                this.submitting = false
            },

            clearStatus() {
                this.success = false
                this.error = false
            },
            onChange(e) {
                const { name, value } = e.target;
                this.task[name] = value;
                this.clearStatus();
            }
        },
        computed: {
            isTitleValid() {
                return this.task.title === '';
            },
            isDescriptionValid () {
                return this.task.description === '';
            },
            isPeriortyValid() {
                return this.task.periorty === '';
            }
        },
    }
</script>

<style scoped>
    form {
        margin-bottom: 2rem;
        max-width: 400px;
        text-align: left;
        margin: auto;
    }

    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color:#d33c40;
    }

    .success-message {
        color: #32a95d;
    }
    .form-item {
        display: flex;
        padding: 10px 0;
    }
    .form-item input {
        box-shadow: none;
    }
    .form-item label {
        width: 150px;
        display: block;
    }
    .has-error {
        border-color: #d33c40;
    }
</style>