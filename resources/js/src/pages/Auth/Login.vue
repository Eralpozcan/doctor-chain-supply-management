<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">{{ $t('general.login') }}</div>

                    <div class="card-body">
                        <div class="row mb-3">
                            <label for="email" class="col-md-4 col-form-label text-md-end">{{ $t('modules.auth.register.form.email.title') }}</label>

                            <div class="col-md-6">
                                <input id="email" v-model="email" type="email" class="form-control" name="email" required autocomplete="email" autofocus>
                            </div>
                        </div>

                        <div class="row mb-3">
                            <label for="password" class="col-md-4 col-form-label text-md-end">{{ $t('modules.auth.register.form.password.title') }}</label>

                            <div class="col-md-6">
                                <input id="password" v-model="password" type="password" class="form-control" name="password" required autocomplete="current-password">
                            </div>
                        </div>

                        <div class="row mb-3">
                            <div class="col-md-6 offset-md-4">
                                <div class="form-check">
                                    <input class="form-check-input" v-model="remember" type="checkbox" name="remember" id="remember">

                                    <label class="form-check-label" for="remember">{{ $t('modules.auth.register.form.remember.title') }}</label>
                                </div>
                            </div>
                        </div>

                        <div class="row mb-0">
                            <div class="col-md-8 offset-md-4">
                                <button type="button" @click.prevent="login" class="btn btn-primary">{{ $t('general.login') }}</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { mapActions } from 'vuex'

export default {
    name: "Auth.Login",
    data(){
        return {
            email: null,
            password: null,
            remember: true,
        }
    },
    methods: {
        ...mapActions('global', [
        'loginUser'
        ]),
        async login(){
            console.log("5")
            const data = {
                email: this.email,
                password: this.password,
                remember: this.remember,
            }
            await axios.get('/sanctum/csrf-cookie').then(async () => {
                await axios.post('/api/auth/login', data).then(()=>{
                    this.loginUser()
                }).catch(({response})=>{
                    if(response.status===422){
                        this.validationErrors = response.data.errors
                    }else{
                        this.validationErrors = {}
                        alert(response.data.message)
                    }
                })
            })
        },
    }
}
</script>

<style scoped>

</style>
