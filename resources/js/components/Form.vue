<template>
</template>

<script>
import FormErrors from './FormErrors.vue';
export default {
    name: 'Form',
    mixins: [FormErrors],
    data(){
        return{
            success: false,
            error: false,
            errorMessage: '',
        }
    },
    methods:{
        post(url, data){
            this.success = false;
            this.error = false;
            axios.post(url,data)
                 .then((res) => {
                     this.onSuccess(res.data.message);
                 })
                 .catch((error) => {
                     if(error.response.status == 422){
                         this.setErrors(error.response.data.errors);
                     }else{
                        this.onFailure(error.response.data.message);
                     }
                     
                 });
        },

        get(url, data){
            this.success = false;
            this.error = false;
            axios.get(url,data)
                 .then((res) => {
                     this.onSuccess(res.data.message);
                 })
                 .catch((error) => {
                     if(error.response.status == 422){
                         this.setErrors(error.response.data.errors);
                     }else{
                        this.onFailure(error.response.data.message);
                     }
                     
                 });
        },

        onSuccess(message){
            this.reset();
            this.success = true;
        },

        onFailure(message){
            this.error = true;
            this.errorMessage = message;
        },

        reset(){
            this.clearAllErrors();

            for(let field in this.formData){
                this.formData[field] = null;
            }
        },
     }
}
</script>