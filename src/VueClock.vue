<template>
  <div class="clock" v-if="hourtime != ''">
    <div class="clock__hours" v-if="isHour">
      <span class="clock__hourtime" v-text="hourtime"></span>
      <span v-text="hours"></span>
    </div>
    <div class="clock__minutes" v-text="minutes" v-if="isMinute"></div>
    <div class="clock__seconds" v-text="seconds" v-if="isSecond"></div>
  </div>
</template>

<script>
import { SECOND, HOUR, getHourTime, getZeroPad } from './filters';

export default {
  props: {
    isHour: {
      type: Boolean,
      default: true
    },
    isMinute: {
      type: Boolean,
      default: true
    },
    isSecond: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      hours: 0,
      minutes: 0,
      seconds: 0,
      hourtime: '',
    };
  },
  mounted() {
    const timer = window.setTimeout(this.updateDateTime, SECOND);
    this.$on('hook:destroyed', () => window.clearTimeout(timer))
  },
  methods: {
    updateDateTime() {
      const now = new Date();
      this.hours = now.getHours();
      this.minutes = getZeroPad(now.getMinutes());
      this.seconds = getZeroPad(now.getSeconds());
      this.hourtime = getHourTime(this.hours);
      this.hours = this.hours % HOUR || HOUR;
      this.$options.timer = window.setTimeout(this.updateDateTime, SECOND);
    },
  },
};
</script>

<style scoped>
.clock {
  background: #fff;
  border: 0.3rem solid #fff;
  border-radius: 0.5rem;
  display: inline-block;
  margin-bottom: 1em;
}

.clock__hours,
.clock__minutes,
.clock__seconds {
  background: linear-gradient(to bottom, #26303b 50%, #2c3540 50%);
  display: inline-block;
  color: #fff;
  font-family: 'Nunito', sans-serif;
  font-size: 3rem;
  font-weight: 300;
  padding: 0.5rem 1rem;
  text-align: center;
  position: relative;
}

.clock__hours {
  border-right: 0.15rem solid #fff;
}

.clock__minutes {
  border-right: 0.15rem solid #fff;
}

.clock__hourtime {
  font-size: 1rem;
  position: absolute;
  top: 2px;
  left: 8px;
}
</style>
