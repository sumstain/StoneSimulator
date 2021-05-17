<template>
    
    <div>
        <br>
        <v-row no-gutters>
        <v-col md = "4" >
        <v-select
            class = "ma-4"
            v-model = "selectedValue"
            :items="cansu"
            item-text = "name"
            item-value = "value"
            label = "칸수"
            return-object
            @change="init()"
        ></v-select>
        <div class="ma-2"><h2 class="display-5" white >{{percent}} %</h2></div> 
        </v-col>
        <v-col md = "4">
            <v-btn class = "ma-4" color="primary" elevation="10" depressed @click="init()" ><v-icon>mdi-refresh</v-icon></v-btn>
        </v-col>


        
        </v-row>
        <v-row  v-model = "rating1" no-gutters>
        <div class="ma-2" v-for = "(item,index) in list1" v-bind:key = "index" ><v-icon large v-bind:color = item.color>{{item.icon}}</v-icon></div>
        <v-btn class="ma-2" color="primary" elevation="10" depressed @click="setRating1(1)" ><v-icon>mdi-hammer</v-icon></v-btn>

        </v-row>
        
        <v-row v-model = "rating2" no-gutters >
        <div class="ma-2" v-for = "(item,index) in list2" v-bind:key = "index"><v-icon large v-bind:color = item.color >{{item.icon}}</v-icon></div>
        <v-btn class="ma-2" color="primary" elevation="10" depressed @click="setRating2(1)" ><v-icon>mdi-hammer</v-icon></v-btn>
        </v-row>
        <v-row v-model = "rating3" no-gutters>
        <div  class="ma-2" v-for = "(item,index) in list3" v-bind:key = "index"><v-icon large v-bind:color = item.color>{{item.icon}}</v-icon></div>
        <v-btn class="ma-2" color="primary" elevation="10" depressed @click="setRating3(1)" ><v-icon>mdi-hammer</v-icon></v-btn>
        </v-row>

        <div class="ma-2" ><h2 class="display-5" white v-html = "result" ></h2></div>


    </div>
</template>
<script>
import failSound from '../assets/break.mp3'
import successSound from '../assets/success.wav'
export default {

    data: () => ({
        rating1: -1,
        rating2: -1,
        rating3: -1,
        selectedValue:{
            name: '6칸',
            value: '6'
        },
        total : 0,
        successValue : 0,
        failValue : 0,
        percent : 75,
        result: '',
        resultList:[
            { text: '당신의 돌은 형편없습니다' }, 
            { text: '당신의 돌은 평균입니다' }, 
            { text:'당신의 돌은 찬란하게 빛이납니다'}],
        list1: [],
        list2: [],
        list3: [],
        cansu: [
            {
                name:'6칸',
                value: '6'
            },
            {
                name:'7칸',
                value: '7'
            },
            {
                name:'8칸',
                value: '8'
            },
            {
                name:'9칸',
                value: '9'
            },
            {
                name:'10칸',
                value: '10'
            }

        ],
        length : 6

    }),
    methods:{
        changeLength(){
            this.length = this.selectedValue.value
            this.clear();
            //this.init();

        },
        setRating1(num){
            var a = this.rating1 + 1;
            var b = this.length;
            if(a == b){
                console.log('full');
                return;
            }
     
            this.rating1 += num
            this.list1[this.rating1].icon = 'mdi-cards-diamond'
            if(!this.isSuccess()){
                this.list1[this.rating1].color = 'gray';
            }
            else{
                this.successValue += 1;
            }
            this.total += 1;


        },
        setRating2(num){
            var a = this.rating2 + 1;
            var b = this.length;
            if(a == b){
                console.log('full');
                return;
            }
            this.rating2 += num
            this.list2[this.rating2].icon = 'mdi-cards-diamond'
            if(!this.isSuccess()){
                this.list2[this.rating2].color = 'gray';
            }
            else{
                this.successValue += 1;
            }
            this.total += 1;
        },
        setRating3(num){
            var a = this.rating3 + 1;
            var b = this.length;
            if(a == b){
                console.log('full');
                return;
            }
            this.rating3 += num
            this.list3[this.rating3].icon = 'mdi-cards-diamond'
            if(!this.isSuccess()){
                this.list3[this.rating3].color = 'gray';
            }
            else{
                this.failValue += 1;
            }
            this.total += 1;
        },
        clear(){
            this.rating1 = -1;
            this.rating2 = -1;
            this.rating3 = -1;
            this.result = '';
            this.total = 0;
            this.successValue = 0;
            this.failValue = 0;
        },
        init(){
            this.length = this.selectedValue.value
            this.percent = 75;
            this.list1 = []; 
            this.list2 = [];
            this.list3 = [];
            this.clear();
                       
            for(let i = 0; i<this.length; i++){
                this.list1.push({icon:'mdi-cards-diamond-outline', color:"blue"});
                this.list2.push({icon:'mdi-cards-diamond-outline', color:"green"});
                this.list3.push({icon:'mdi-cards-diamond-outline', color:"red"});
                     
            }

        },
        playFailSound(){
            var audio = new Audio(failSound);
            audio.volume = 0.7;
            audio.play();
        },
        playSuccessSound(){
            var audio = new Audio(successSound);
            audio.volume = 0.7;
            audio.play();
        },
        isSuccess() {
            var d = Math.random();
            if (d < (this.percent / 100)){
                if(this.percent != 25){
                    this.percent -= 10;
                }
                this.playSuccessSound();             
                return true;
            }
                
            else{
                if(this.percent != 75){
                    this.percent += 10;
                }
                
                this.playFailSound();
                return false;
            }

        },



        
    },
    created() {
        if(failSound && successSound)
            this.init();

    },
    watch:{
        total:function(newTotal){
            console.log('total>>' +newTotal);
            console.log('success>>' + this.successValue);
            if(newTotal == this.length *3){
                this.result += this.resultList[0].text;
                // const score = (this.successValue)/newTotal;
                // console.log(score);
                // if(score <= 0.5)
                //     this.result += this.resultList[0].text;
                // else if (score > 0.5 && score < 0.8){
                //     this.result += this.resultList[1].text;
                // }
                // else{
                //     this.result += this.resultList[2].text;
                // }
                
            }

        }
        
    }


}
</script>

<style scoped>

</style>
