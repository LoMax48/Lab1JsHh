<template>
    <input type="number" v-model="inputValue" />
</template>

<script>
export default {
    name: "InputPhoneNumberComponent",
    props: ['modelValue'],
    watch: {
        inputValue: function (value) {
            if (value.length == 11) {
                document.getElementById('phone').classList.remove("is-invalid");
            } else {
                document.getElementById('phone').classList.add("is-invalid");
            }
        }
    },
    computed:{
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