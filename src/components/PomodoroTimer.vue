import bgImage from '../assets/bg.jpeg'

<template>
    <p>Number of focus sessions completed: {{ sessionCount }}</p>
    <div class="input-box">
      <label for="time-input">Productivity Session:</label>
      <div class="input-container">
        <input type="number" min="0" max="23" placeholder="HH" v-model="prod_hours" class="hour-input" />
        <span>:</span>
        <input type="number" min="0" max="59" placeholder="MM" v-model="prod_minutes" class="minute-input" />
      </div>
    </div>
    <div class="input-box">
      <label for="time-input">Break Session:</label>
      <div class="input-container">
        <input type="number" min="0" max="23" placeholder="HH" v-model="break_hours" class="hour-input" />
        <span>:</span>
        <input type="number" min="0" max="59" placeholder="MM" v-model="break_minutes" class="minute-input" />
      </div>
    </div>
    <div class="pomodoro-timer">
        <div class="pomodoro-timer__background">
            <div v-if="!isRunning" class="pomodoro-timer__buttons">
                <button @click="startSession(productivity_mins(), true)" class="pomodoro-timer__button pomodoro-timer__button--25">Start Productivity Session</button>
            </div>
            <div class="pomodoro-timer__counter">
                <h2 style='color:black;'>{{ time }}</h2>
                <button @click="stopSession" class="pomodoro-timer__button pomodoro-timer__button--stop">Stop</button>
            </div>
        </div>
    </div>
    <div class="bottom-box">
      <div>
        <h1>Bitcoin Price Index</h1>
        <section v-if="errored">
          <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
        </section>

        <section v-else>
          <div v-if="loading">Loading...</div>

          <div
            v-else
            v-for="currency in info" :key="currency.code"
            class="currency"
          >
            {{ currency.description }}:
            <span class="lighten">
              <span v-html="currency.symbol"></span>{{ currency.rate_float }}
            </span>
          </div>
        </section>
      </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
        return {
            isRunning: false,
            time: "",
            timer: null,
            sessionCount: 0,
            prod_hours: '',
            prod_minutes: '',
            break_hours:'',
            break_minutes:'',
            info: null,
            loading: true,
            errored: false
        };
  },
  beforeRouteLeave(to, from, next) {
    if (from.name === 'PomodoroTimer') {
      if (confirm('Are you sure you want to log out?')) {
        // User confirmed, proceed to the next route
        next();
      } else {
        // User cancelled, stay on the current route
        next(false);
      }
    } else {
      // Not navigating from the PomodoroTimer component, proceed to the next route
      next();
    }
  },
  mounted () {
    axios
      .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => (this.info = response.data.bpi))
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  },
  methods: {
        productivity_mins() {
          this.prod_hours = parseInt(this.prod_hours)
          this.prod_minutes = parseInt(this.prod_minutes)
          if(Number.isNaN(this.prod_hours)){
            this.prod_hours = 0;
          }
          const total_mins = parseInt(this.prod_hours)*60 + parseInt(this.prod_minutes)
          return total_mins
        },
        break_mins() {
          this.break_hours = parseInt(this.break_hours)
          this.break_minutes = parseInt(this.break_minutes)
          if(Number.isNaN(this.break_hours)){
            this.break_hours = 0;
          }
          const total_mins = parseInt(this.break_hours)*60 + parseInt(this.break_minutes)
          return total_mins
        },
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
                    if(this.isSession) {
                      this.sessionCount++;
                      this.startSession(this.break_mins(), false);
                    }else{
                      this.time = "";
                    }
                }
                
            },1000);
        },
    },

};
</script>

<style scoped>
.bottom-box {
  background-color: #f2f2f2;
  padding: 20px;
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  width: 30%;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  padding-bottom: 10px;
}

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

.input-box {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
  align-items: center;
}

label {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
}

.input-container {
  display: flex;
  align-items: center;
}

input[type="number"] {
  border: none;
  border-radius: 4px;
  font-size: 16px;
  font-weight: bold;
  padding: 10px;
  text-align: center;
  width: 50px;
  background-color: #f5f5f5;
  margin-right: 10px;
}

input[type="number"]:focus {
  outline: none;
  box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.3);
}

span {
  font-size: 20px;
  margin-right: 10px;
}

</style>
