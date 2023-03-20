import bgImage from '../assets/bg.jpeg'

<template>
    <p>Number of focus sessions completed: {{ sessionCount }}</p>
    <div class="pomodoro-timer">
        <div class="pomodoro-timer__background">
            <div v-if="!isRunning" class="pomodoro-timer__buttons">
                <button @click="startSession(2, true)" class="pomodoro-timer__button pomodoro-timer__button--25">Start 25 min session</button>
                <button @click="startSession(5, true)" class="pomodoro-timer__button pomodoro-timer__button--50">Start 50 min session</button>
            </div>
            <div class="pomodoro-timer__counter">
                <h2 style='color:black;'>{{ time }}</h2>
                <button @click="stopSession" class="pomodoro-timer__button pomodoro-timer__button--stop">Stop</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  data() {
        return {
            isRunning: false,
            time: "",
            timer: null,
            sessionCount: 0,
        };
  },
  methods: {
        startSession(minutes, isSession) {
            this.minutes = minutes;
            this.isRunning = true;
            this.time=`${minutes}:00`
            this.isSession = isSession;
            this.startTimer();
        },
        stopSession() {
            clearInterval(this.timer);
            this.isRunning = false;
            this.time = "";
        },
        pad(value) {
             return value < 10 ? `0${value}` : value;
        },
        startTimer() {
            this.isRunning = true;
            let seconds = this.minutes * 60;
            console.log(this.sessionCount);
            this.timer = setInterval(() => {
                seconds--;
                let m = Math.floor(seconds / 60);
                let s = seconds % 60;
                this.time = `${m.toString().padStart(2, "0")}:${s.toString().padStart(2, "0")}`;
                if (seconds === 0) {
                    clearInterval(this.timer);
                    this.isRunning = false;
                    if(this.minutes == 2){
                        this.time = "01:00";
                        if(this.isSession) {this.sessionCount++;}
                        this.startSession(1, false);
                        
                    } else if (this.minutes === 5){
                        this.time = "03:00";
                        if(this.isSession) {this.sessionCount++;}
                        this.startSession(3, false);

                    } 
                }
                
            },1000);
        },
    },

};
</script>

<style scoped>
.pomodoro-timer {
  text-align: center;
  font-size: 2em;
  color: #fff;
  position: relative;
}

.pomodoro-timer__background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: bgImage;
  background-size: cover;
  background-position: center;
  opacity: 0.5;
  z-index: -1;
}

.pomodoro-timer__buttons {
  display: flex;
  justify-content: center;
}

.pomodoro-timer__button {
  margin: 0 10px;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  background-color: #5cb85c;
  color: #fff;
  font-size: 1em;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.pomodoro-timer__button:hover {
  background-color: #4cae4c;
}

.pomodoro-timer__button--25 {
  background-color: #f0ad4e;
}

.pomodoro-timer__button--25:hover {
  background-color: #eea236;
}

.pomodoro-timer__button--50 {
  background-color: #d9534f;
}

.pomodoro-timer__button--50:hover {
  background-color: #d43f3a;
}

.pomodoro-timer__button--stop {
  margin-top: 20px;
  padding: 5px 10px;
  background-color: #d9534f;
}

.pomodoro-timer__button--stop:hover {
  background-color: #d43f3a;
}

.pomodoro-timer__counter {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

</style>
