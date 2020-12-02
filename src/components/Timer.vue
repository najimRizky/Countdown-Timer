<template>
    <div class="timer1" >
        <v-container>
            <v-row text="center">
                <v-col align="center">
                <h2>⏳ Countdown Timer ⏳</h2>
                </v-col>
            </v-row>
            <v-row id="timer1">
                <v-spacer></v-spacer>
                <v-col cols="10" align="center">
                    <v-row>
                        <v-col cols="3" style="padding: 0px"><img src="@/assets/segitigaUp.png" @click="addJam()"></v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="3" style="padding: 0px"><img src="@/assets/segitigaUp.png" @click="addMenit()"></v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="3" style="padding: 0px"><img src="@/assets/segitigaUp.png" @click="addDetik()"></v-col>
                    </v-row>
                    <v-row>
                        <v-col cols="3"><div>{{jam || '00'}}</div></v-col>
                        <v-col cols="1">:</v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="3"><div>{{menit || '00'}}</div></v-col>
                        <v-col cols="1">:</v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="3"><div>{{detik || '00'}}</div></v-col>
                    </v-row>
                    <v-row style="font-weight: normal; font-size: 12px;">
                        <v-col cols="3" style="padding: 0px"><span>Hours</span></v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="3" style="padding: 0px"><span>Minutes</span></v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="3" style="padding: 0px"><span>Seconds</span></v-col>
                    </v-row>
                    <v-row>
                        <v-col cols="3"><img src="@/assets/segitigaDown.png" @click="decJam()"></v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="3"><img src="@/assets/segitigaDown.png" @click="decMenit()"></v-col>
                        <v-spacer></v-spacer>
                        <v-col cols="3"><img src="@/assets/segitigaDown.png" @click="decDetik()"></v-col>
                    </v-row>
                    <v-row v-if="!timerStats">
                        <v-col cols="12">
                            <v-btn  color="green" v-on:click="startTimer()">Start</v-btn>
                        </v-col>
                    </v-row>
                    <v-row v-if="timerStats">
                        <v-col cols="12">
                            <v-btn color="green" style="margin:0px 5px"  v-if="pauseStats" v-on:click="startTimer()">Start</v-btn>
                            <v-btn color="yellow" style="margin:0px 5px" @click="jedaTimer()" v-if="!pauseStats">Pause</v-btn>
                            <v-btn color="red" style="margin:0px 5px" @click="resetTimer()">Reset</v-btn>
                        </v-col>
                    </v-row>
                </v-col>
                <v-spacer></v-spacer>
            </v-row>
            <v-row >
                <v-col align="center">
                    <h1 v-if="timerStats && !pauseStats">Timer is running</h1>
                    <h1 v-if="finishStats">Timer is finish</h1>
                    <h1 v-if="pauseStats">Paused!</h1>
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>

<script>
    export default {
        data: () => ({
            jam: 0, menit: 0, detik: 0,
            timerStats: false,
            pauseStats: false,
            finishStats: false,
            tmpJam: 0, tmpMenit: 0, tmpDetik: 0,
            tmp2Jam: 0, tmp2Menit: 0, tmp2Detik: 0,
            i:0,
            pauseIndicator: 0,
            currThn:0,
            currBln:0,
            currTgl:0,
            currJam:0,
            currMenit:0,
            currDetik:0,
        }),
        computed: {
            _detiks: () => 1000,
            _menits(){
                return this._detiks * 60;
            },
            _jams(){
                return this._menits * 60;
            }
        },
        methods: {
            addJam(){
                this.tmpJam += 1;
                this.jam = this.formatNum(this.tmpJam)
            },
            addMenit(){
                if(this.tmpMenit < 59){
                    this.tmpMenit += 1;
                }else{
                    this.tmpJam += 1
                    this.tmpMenit = 0;
                }
                this.menit = this.formatNum(this.tmpMenit)
                this.jam = this.formatNum(this.tmpJam)    
                this.detik = this.formatNum(this.tmpDetik)    
            },
            addDetik(){
                if(this.tmpDetik < 59){
                    this.tmpDetik += 1;
                }else{
                    this.tmpMenit += 1
                    this.tmpDetik = 0;
                }
                this.menit = this.formatNum(this.tmpMenit)
                this.jam = this.formatNum(this.tmpJam)    
                this.detik = this.formatNum(this.tmpDetik)  
            },
            decJam(){
                if(this.tmpJam >0){
                    this.tmpJam -= 1;
                }
                this.jam = this.formatNum(this.tmpJam)    
            },
            decMenit(){
                if(this.tmpMenit >0){
                    this.tmpMenit -= 1;
                }
                this.menit = this.formatNum(this.tmpMenit)
            },
            decDetik(){
                if(this.tmpDetik >0){
                    this.tmpDetik -= 1;
                }
                this.detik = this.formatNum(this.tmpDetik)  
            },
            startTimer(){
                if(this.pauseStats == true){
                    this.tmpJam = this.tmp2Jam;
                    this.tmpMenit = this.tmp2Menit;
                    this.tmpDetik = this.tmp2Detik;
                    this.pauseStats = false;
                }
                if(this.tmpJam > 0 || this.tmpMenit > 0 || this.tmpDetik > 0){
                    this.showRemaining()
                }
            },
            formatNum: num => (num < 10 ? '0' + num : num),
            showRemaining(){
                this.timerStats = true;
                this.pauseStats = false;
                const timerz = setInterval(() => {
                    const now = new Date();
                    if(this.i==0){
                        //console.log('sini')
                        this.currThn = now.getFullYear();
                        this.currBln = now.getMonth();
                        this.currTgl = now.getDate();
                        this.currJam = now.getHours() + this.tmpJam;
                        this.currMenit = now.getMinutes() + this.tmpMenit;
                        this.currDetik = now.getSeconds()+ this.tmpDetik;     
                        this.i += 1;
                        //console.log('Curr jam ',this.currJam, this.currMenit, this.currDetik)
                    }
                    const end = new Date(this.currThn,this.currBln,this.currTgl,this.currJam, this.currMenit, this.currDetik);
                    const distance = end.getTime() - now.getTime();
                    //console.log('Distance: ', distance)
                    if(distance < 0){
                        clearInterval(timerz);
                        this.tmpJam = 0;
                        this.tmpMenit = 0;
                        this.tmpDetik = 0;
                        this.i = 0;
                        if(this.pauseStats == false) {
                            this.finishAlert();
                            this.timerStats = false;
                        }
                        return;
                    }
                    const jams = Math.floor(distance / this._jams);
                    const menits = Math.floor((distance % this._jams) / this._menits);
                    const detiks = Math.floor((distance % this._menits) / this._detiks);
                    this.tmp2Jam = jams;
                    this.tmp2Menit =  menits;
                    this.tmp2Detik = detiks;
                    this.jam = this.formatNum(jams);
                    this.menit = this.formatNum(menits);
                    this.detik = this.formatNum(detiks);
                    console.log('pause', this.tmp2Jam, this.tmp2Menit, this.tmp2Detik)
                }, 1000);
            },
            resetTimer(){
                clearInterval(this.timerz);
                this.tmpJam = 0; this.jam = this.formatNum(this.tmpJam);
                this.tmpMenit = 0; this.menit = this.formatNum(this.tmpMenit);
                this.tmpDetik = 0; this.detik = this.formatNum(this.tmpDetik);
                this.i = 0;
                this.timerStats = false;
                this.pauseStats = false;
                return;
            },
            jedaTimer(){
                clearInterval(this.timerz);
                //const pauseNow = new Date();
                this.i = 0;
                this.tmpJam = 0;
                this.tmpMenit = 0;
                this.tmpDetik = 0;
                this.pauseStats = true;
                return;
            },
            finishAlert(){
                    this.finishStats=true
                setTimeout(()=>{
                    this.finishStats=false
                },2000)
            }
        }
    }
</script>

<style scoped>
    img{
        width: 30px;
        cursor: pointer;
    }
    #timer1{
        font-size: 30px;
        font-weight: bold;
    }
    .timer1{
    justify-content: center;
    align-items: center;
    display: flex;
    height: 100%;
    }
</style>