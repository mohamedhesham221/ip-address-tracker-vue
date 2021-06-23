<template>
  <div>
    <div class="errorMsg" :class="{error : showErr}">
      Invalid IP Address <br />
      type a valid IP Address
    </div>
      <div class="top-section">
      <h2>{{ title }}</h2>
      <form @submit.prevent="getIp();">
          <input type="text" placeholder="Search for any IP address" v-model="ip"
          aria-label="Search for any IP address" />
          <input type="submit" aria-label="submit" value="" />
      </form>
      <div class="ip-result">
          <div>
              <p>IP Address</p>
              <p>{{ ipAddress }}</p>
          </div>
          <div>
              <p>Location</p>
              <p>{{ city }}</p>
          </div>
          <div>
          <p>Timezone</p>
          <p> {{ timezone }}</p>
            </div>
          <div>
              <p>ISP</p>
              <p>{{ isp }}</p>
          </div>
      </div>
      <div></div>
  </div>

  </div>
</template>

<script>
import EventBus from '../main';
import Api from '../../config';

export default {

  name: 'ip-tracker',
  data() {
    return {
      title: 'IP Address Tracker',
      ip: '66.249.73.40',
      arrTrackers: null,
      ipAddress: null,
      city: null,
      timezone: null,
      isp: null,
      showErr: false,

    };
  },
  created() {
    this.getIp();
  },
  methods: {
    getIp() {
      const url = `https://geo.ipify.org/api/v1?apiKey=${Api}&ipAddress=${this.ip}`;
      // eslint-disable-next-line no-return-assign
      fetch(url).then((response) => response.json())
        // eslint-disable-next-line no-return-assign
        .then((result) => {
          EventBus.$emit('data', result);
          // eslint-disable-next-line no-return-assign
          this.arrTrackers = result;
          this.ipAddress = this.arrTrackers.ip;
          this.city = this.arrTrackers.location.city;
          this.timezone = `UTC ${this.arrTrackers.location.timezone}`;
          this.isp = this.arrTrackers.isp;
          this.showErr = false;
        })
        .catch((error) => {
          this.showErr = true;
          this.ipAddress = 'Invalid';
          this.city = 'Invalid';
          this.timezone = 'Invalid';
          this.isp = 'Invalid';
          console.log(error);
        });
    },
  },
};
</script>

<style scoped>
  .top-section {
    background-image: url("../assets/pattern-bg.png");
    padding: 20px 20px 0;
    height: 240px;
}
h2 {
    color: var(--white);
    font-size: calc(1rem + 1px);
    margin-bottom: 15px;
}
form {
    display: flex;
    justify-content: center;
}
input {
    border: none;
    outline: none;
    height: 50px;
    width: 200px;
}
input::placeholder {
    color: var(--darkGray);
    font-size: calc(1rem - 7px);
}
input[type=text] {
    padding-left: 15px;
    border-top-left-radius: 12px;
    border-bottom-left-radius: 12px;
}
input[type=submit] {
    margin-left: -4px;
    width: 50px;
    border-top-right-radius: 12px;
    border-bottom-right-radius: 12px;
    background: no-repeat center url("../assets/icon-arrow.svg"),var(--veryDarkGray);
    cursor: pointer;
}
.ip-result {
    background-color: var(--white);
    border-radius: 15px;
    margin: 30px auto 0;
    padding: 20px 0;
    width: 250px;
}
.ip-result div {
    margin-bottom: 15px;
    border: none;
}
.ip-result div p:first-child {
    color: var(--darkGray);
    font-size: calc(1rem - 10px);
    letter-spacing: 2px;
}

.errorMsg {
  width: 100%;
  height: max-content;
  padding: 5px 0;
  position: absolute;
  top: -40px;
  background-color: #be1111;
  color: var(--white);
  font-size: calc(1rem - 5px);
  transition: all .3s ease-in;
}
.error {
  top: 0;
}
@media (min-width: 1024px) and (max-width: 1440px) {
  .ip-result {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 615px;
    height: 100px;
    margin-top: 55px;
}
  .ip-result div {
    margin: 0 20px;
    height: 100%;

  }
  .ip-result div:not(:first-child) {
    border-left: 2px solid var(--darkGray);
    padding-left: 10px;
  }
  input[type=text] {
    width: 330px;
  }
  .ip-result div p:first-child {
    margin-bottom: 8px;
  }
}
</style>
