<template>
<div class="login bg-light min-vh-100 d-flex flex-row align-items-center">
    <div class="container">
        <div class="row justify-content-center  ">
            <div class="col-sm-12 col-md-8 col-lg-6  flex">
                <div class="card w-[80%]">
                    <div class="card-header">
                        <div class="card-title p-4 text-3xl font-bold">Login User</div>
                    </div>
                    <div class="card-body">
                        <form novalidate @submit.prevent="loginUser">
                            <div class="form-group">
                                <input type="text" :class="`form-control ${
                        errors.username ? 'is-invalid' : ''
                      }`" placeholder="Username" v-model="username" />
                                <div class="invalid-feedback">{{ errors.username }}</div>
                            </div>
                            <div class="form-group">
                                <input type="password" :class="`form-control ${
                        errors.password ? 'is-invalid' : ''
                      }`" placeholder="Password" v-model="password" />
                                <div class="invalid-feedback">{{ errors.password }}</div>
                            </div>
                            <button class="btn bg-orange-400 outline outline-orange-500 form-control hover:bg-orange-300">
                                Login
                            </button>
                        </form>
                    </div>

                </div>
                <CCard class="text-white bg-primary py-5 flex " style="width:60%">
                    <CCardBody class="text-center">
                        <div>
                            <h2>Sign up</h2>
                            <p>
                                Lorem ipsum dolor sit amet, consectetur adipisicing elit,
                                sed do eiusmod tempor incididunt ut labore et dolore magna
                                aliqua.
                            </p>
                            <RouterLink :to="{ name: 'Register' }">
                                <CButton color="light" variant="outline" class="mt-3">
                                    Register Now!
                                </CButton>
                            </RouterLink>

                        </div>
                    </CCardBody>
                </CCard>
            </div>

        </div>
    </div>

</div>
</template>

<script>
import validateLoginInput from "../validation/validateLoginInput.js";
export default {
    data() {
        return {
            username: "",
            password: "",
            errors: {},
        };
    },
    methods: {
        loginUser() {
            let credentials = {
                username: this.username,
                password: this.password,
            };

            const {
                isInvalid,
                errors
            } = validateLoginInput(credentials);

            if (isInvalid) {
                this.errors = errors;
            } else {
                this.errors = {};
                // login code goes here
                let lsUsers = localStorage.users;
                lsUsers = JSON.parse(lsUsers);
                let usernameIndex = lsUsers.findIndex(
                    (user) => user.username === credentials.username
                );
                if (usernameIndex > -1) {
                    let passwordIndex = lsUsers.findIndex(
                        (user) => user.password === credentials.password
                    );

                    if (passwordIndex > -1) {
                        let activeUser = lsUsers.find(
                            (user) => user.username === credentials.username
                        );
                        localStorage.setItem("activeUser", JSON.stringify(activeUser));
                        console.log(123);
                        this.$router.push("/");
                    } else {
                        this.errors.password = "Password does not match!";
                    }
                } else {
                    this.errors.username = "Username does not exist!";
                }
            }
        },
    },
};
</script>

<style>
.login {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
</style>
