<template>
    
        
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Register</div>

                    <div class="card-body">
                        <form aria-label="Register" @submit.prevent="registerPost()">
                           

                            <div class="form-group row">
                                <label for="name" class="col-md-4 col-form-label text-md-right">Name</label>

                                <div class="col-md-6">
                                    <input id="name" type="text" class="form-control" name="name"
                                           v-model="registerData.name" required autofocus>
                                    <span v-if="hasErrors.name" class="help-block">
                                        <strong>{{errorMessage.name}}</strong>
                                    </span>

                                    
                                </div>
                            </div>

                            <div class="form-group row">
                                <label for="email" class="col-md-4 col-form-label text-md-right">Email</label>

                                <div class="col-md-6">
                                    <input id="email" type="email" class="form-control" name="email"
                                           v-model="registerData.email" required>
                                    <span v-if="hasErrors.email" class="help-block">
                                        <strong>{{errorMessage.email}}</strong>
                                    </span>

                                   
                                </div>
                            </div>

                            <div class="form-group row">
                                <label for="password" class="col-md-4 col-form-label text-md-right">Password</label>

                                <div class="col-md-6">
                                    <input id="password" type="password" class="form-control" name="password"
                                           v-model="registerData.password" required>
                                    <span v-if="hasErrors.password" class="help-block">
                                        <strong>{{errorMessage.password}}</strong>
                                    </span>

                                   
                                </div>
                            </div>

                            <div class="form-group row">
                                <label for="password-confirm" class="col-md-4 col-form-label text-md-right">Confirm Password</label>

                                <div class="col-md-6">
                                    <input id="password-confirm" type="password" class="form-control"
                                           name="password_confirmation" v-model="registerData.password_confirmation"
                                           required>
                                </div>
                            </div>

                            <div class="form-group row mb-0">
                                <div class="col-md-6 offset-md-4">

                                    <button type="submit" class="btn btn-primary">
                                        
                                        <hollow-dots-spinner v-if="clickButton.regitration"
                                              :animation-duration="500"
                                              :dot-size="15"
                                              :dots-num="3"
                                              :color="'#fff'"
                                            />

                                        <span v-if="clickButton.text"> Register </span>
                                    </button>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
                
            </div>
        </div>
    </div>
</template>

<script>
    import {HollowDotsSpinner} from 'epic-spinners'
    export default{
        components: {
          HollowDotsSpinner
        },
        data(){
            return{
                registerData:{
                    name:'',
                    email:'',
                    password:'',
                    password_confirmation:''
                },
                hasErrors:{
                    name:false,
                    email:false,
                    password:false
                },
                errorMessage:{
                    name:null,
                    email:null,
                    password:null
                },
                clickButton:{
                    regitration:false,
                    text:true
                }
                //passwordMatch:null
            }
        },
        methods:{
            registerPost(){
                
                var _this = this
                var vm = this.hasErrors
                var _vm = this.errorMessage
                var sid = this.clickButton
                sid.regitration = true
                sid.text = false
                axios.post('register', _this.registerData)
                .then(function (response) {
                  
                    swal({
                      title: "Good Job",
                      text: "Registration Success",
                      icon: "success",
                      button: "Okay",
                    })
                    .then((willDelete) => {
                      if (willDelete) {
                        window.location.href = '/home';
                      } 
                    });
                })
                .catch(function (error) {
                    var errors = error.response
                    // console.log(errors.data.errors.password[0]);
                    if(errors.statusText === 'Unprocessable Entity'){
                        if(errors.data){
                            if(errors.data.errors.name){
                               vm.name = true
                               _vm.name = _.isArray(errors.data.errors.name) ? errors.data.errors.name[0]: errors.data.errors.name
                            }
                            if(errors.data.errors.email){
                               vm.email = true
                               _vm.email = _.isArray(errors.data.errors.email) ? errors.data.errors.email[0]: errors.data.errors.email
                            }
                            if(errors.data.errors.password){
                               vm.password = true
                               _vm.password = _.isArray(errors.data.errors.password) ? errors.data.errors.password[0]: errors.data.errors.password
                            }
                        }
                    }
                });
            }
        }
        
    }
</script>

<style>
    .help-block {
        color: red;
    }
</style>