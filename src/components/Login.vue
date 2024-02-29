<template>
    <div class="container">
        <div class="register-form" v-if="!isLoggedIn">
            <h2 class="name-form">Registration</h2>
            <form class="form-field-container" @submit.prevent="register">
                <div>
                    <!-- <label for="username">Username</label> -->
                    <input
                        class="input-data"
                        placeholder="Username"
                        id="username"
                        v-model="username"
                        required
                    />
                    <p v-if="errors.username">{{ errors.username }}</p>
                </div>
                <div>
                    <!-- <label for="email">Email</label> -->
                    <input
                        class="input-data"
                        placeholder="Email"
                        id="email"
                        v-model="email"
                        type="email"
                        required
                    />
                    <p v-if="errors.email">{{ errors.email }}</p>
                </div>
                <div>
                    <!-- <label for="password">Password</label> -->
                    <input
                        class="input-data"
                        placeholder="Password"
                        id="password"
                        v-model="password"
                        type="password"
                        required
                    />
                    <p v-if="errors.password">{{ errors.password }}</p>
                </div>
                <div>
                    <!-- <label for="confirmPassword">Confirm Password</label> -->
                    <input
                        class="input-data"
                        placeholder="Confirm Password"
                        id="confirmPassword"
                        v-model="confirmPassword"
                        type="password"
                        required
                    />
                    <p v-if="errors.confirmPassword">
                        {{ errors.confirmPassword }}
                    </p>
                </div>
                <button class="btn" type="submit">Register</button>
            </form>
        </div>
        <div class="login-form" v-if="isLoggedIn">
            <h2 class="name-form">Login</h2>
            <form @submit.prevent="login">
                <div>
                    <!-- <label for="loginUsername">Username</label> -->
                    <input
                        class="input-data"
                        placeholder="Username"
                        id="loginUsername"
                        v-model="loginUsername"
                        required
                    />
                </div>
                <div>
                    <!-- <label for="loginPassword">Password</label> -->
                    <input
                        class="input-data"
                        placeholder="Password"
                        id="loginPassword"
                        v-model="loginPassword"
                        type="password"
                        required
                    />
                </div>
                <p v-if="loginError">{{ loginError }}</p>
                <button class="btn" type="submit">Login</button>
            </form>
        </div>
        <button @click="onSwitchBtn" class="swith-btn">
            {{ isLoggedIn ? switchBtnRegister : switchBtnLogin }}
        </button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            username: "",
            email: "",
            password: "",
            confirmPassword: "",
            loginUsername: "",
            loginPassword: "",
            errors: {},
            loginError: "",
            switchBtnLogin: "switch to login",
            switchBtnRegister: "switch to register",
            isLoggedIn: false,
        };
    },
    methods: {
        register() {
            this.errors = {};
            if (
                !this.username ||
                !this.email ||
                !this.password ||
                !this.confirmPassword
            ) {
                this.errors = {
                    username: "Username is required",
                    email: "Email is required",
                    password: "Password is required",
                    confirmPassword: "Confirm Password is required",
                };
                return;
            }
            const usernameRegex = /^[a-zA-Z0-9]{5,15}$/;
            if (!usernameRegex.test(this.username)) {
                this.errors.username =
                    "Tên phải từ 5-15 kí tự, không có khoảng trắng, chỉ là số và chữ, không có kí tự đặc biệt";
                return;
            }
            const emailRegex =
                /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
            if (!emailRegex.test(this.email)) {
                this.errors.email = "Email is not valid";
                return;
            }
            const passwordRegex =
                /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{6,}$/;
            if (!passwordRegex.test(this.password)) {
                this.errors.password =
                    "Password must be at least 6 characters, contain at least 1 number, 1 lowercase letter, 1 uppercase letter, and 1 special character";
                return;
            }
            if (this.password !== this.confirmPassword) {
                this.errors.confirmPassword = "Passwords do not match";
                return;
            }
            sessionStorage.setItem("username", this.username);
            sessionStorage.setItem("password", this.password);
            this.isLoggedIn = true;
            this.errors = {};
            this.loginUsername = "";
            this.loginPassword = "";
        },
        login() {
            this.loginError = "";
            const username = sessionStorage.getItem("username");
            const password = sessionStorage.getItem("password");
            if (
                this.loginUsername !== username ||
                this.loginPassword !== password
            ) {
                this.loginError =
                    "Login failed, please check your account and password";
                return;
            }
            this.isLoggedIn = true;
            this.errors = {};
            this.loginUsername = "";
            this.loginPassword = "";
            alert("Logged in successfully!");
        },
        onSwitchBtn() {
            this.isLoggedIn = !this.isLoggedIn;
        },
    },
};
</script>

<style scoped>
.container {
    background-color: #3aaf9f;
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;
}

.register-form,
.login-form {
    position: relative;
    background-color: #fff;
    height: 500px;
    width: 400px;
    border-radius: 20px;
    box-shadow: #2a7a70 8px 8px 8px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.form-field-container {
    margin-top: 30px;
}

.form-field-container div {
    display: flex;
    justify-content: space-between;
}

.name-form {
    position: absolute;
    top: 40px;
    color: #44ad9c;
    font-size: 30px;
    font-weight: 600;
}

.input-data {
    font-size: 20px;
    outline: none;
    border: none;
    background-color: #eeeeee;
    margin: 10px 0;
    padding: 12px 20px;
    border-radius: 4px;
}

.input-data::placeholder {
    color: #666;
}

.btn {
    border: none;
    outline: none;
    margin-top: 20px;
    background-color: #3aaf9f;
    color: #fff;
    border-radius: 20px;
    font-size: 20px;
    padding: 10px 70px;
}

.swith-btn {
    position: absolute;
    bottom: 150px;
    margin-left: 210px;
    border: none;
    outline: none;
    background-color: #fff;
    color: #394760;
    cursor: pointer;
}
</style>
