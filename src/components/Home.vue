<template>
  <div class="center" style="margin-top: 200px;">
      <div class="col-md-4">
        <loader v-if="showLoader"></loader>
        <div class="col-12">
            <input type="text" v-model="name" class="form-control reset-border" placeholder="Enter Name">
        </div>
        <div class="col-12" style="padding-top: 10px;">
            <input type="text" v-model="code" class="form-control reset-border" placeholder="Enter Code (Q1001)">
        </div>
        <div class="col-12">
            <div style="text-align: center;color: red;" v-if="isIncorrectCode">Incorrect Code</div>
        </div>
        <div class="col-12">
            <button @click="validateCode" style="margin-top: 10px;" class="btn btn-primary btn-block reset-border" :disabled="disable">SUBMIT</button>
        </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios';
import Loader from './Loader';

export default {
    components:{
        Loader
    },
    data(){
        return{
            name: '',
            code: '',
            isIncorrectCode: false,
            showLoader: false,
            quiz: ''
        }
    },
    methods:{
        validateCode: function(){
            
            this.showLoader = true; 
            axios.get('./static/json/questions.json')
            .then((response) => {

                for(let data of response.data){
                    if(data.code == this.code){
                        this.quiz = data.quiz;

                        this.$router.push({name: 'Quiz', params:{quizData: this.quiz}})
                        break;
                    }
                }

                if(this.quiz == ''){
                    this.isIncorrectCode = true;
                }

                this.showLoader = false;
                console.log(this.quiz);

            })
            .catch((error) => {
                console.log(error);
                this.showLoader = false;
            });

        }
    },
    computed:{
        'disable': function(){
            if(this.code.length < 4 || this.name.length < 4){
                return true;
            }

            return false;
        }
    }

}
</script>

<style scoped>

    .center{
        display: flex;
        justify-content: center;
    }

    .reset-border{
        border-radius: 0;
    }

</style>
