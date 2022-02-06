<template>
    <input v-bind:type="this.inputType" v-model="inputValue" />
</template>

<script>
export default {
    name: "InputComponent",
    props: ['inputType', 'modelValue'],
    computed: {
        inputValue: {
            get() {
                return this.modelValue;
            },
            set(value) {
                this.$emit('update:modelValue', value);
            }
        }
    }
}
</script>