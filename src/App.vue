<template>
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <login-form ref="login" />
</template>

<script>
import Login from "./components/Login.vue";

export default {
    name: "App",
    components: {
        LoginForm: Login,
    },
    data() {
        return {};
    },
    methods: {
        onchange() {
            console.log("you click the button ");
            console.log(this.$refs.hello);
        },
    },
};
</script>
