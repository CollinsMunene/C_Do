<template>
    <form @submit="onSubmit" @submit.prevent="onSubmit">
        <label for="new-todo-input">
            What needs to be done?
        </label>
        <input type="text" id="new-todo-input" v-model="label" v-model.lazy="label" v-model.trim="label" name="new-todo" autocomplete="off"/>

        <button type="submit">
            Add
        </button>
    </form>
</template>

<script>
export default {
    data(){
        return{
            label:""
        }
    },
    methods: {
    onSubmit(){
        if(this.label === ""){
            return;
        }
        this.$emit("todo-added", this.label);
        this.label = ""
        console.log("form submitted", this.label);
    },
  },
}
</script>