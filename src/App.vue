<template>
  <div id="app">
    <div class="main">
      <div class="calendar-holder">
        <calendar :events="events" />
      </div>
      <div class="form-holder">
        <h3>Schedule an event</h3>
        <event-form />
      </div>
    </div>
  </div>
</template>

<script>
import Calendar from "./components/Calendar.vue";
import EventForm from "./components/EventForm.vue";
import Pusher from "pusher-js";

export default {
  name: "app",
  components: {
    Calendar,
    EventForm
  },
  data() {
    return {
      events: []
    };
  },
  created() {
    const pusher = new Pusher("3fd8be40d2fb509fc8b4", {
      cluster: "eu",
      encrypted: true
    });
    const channel = pusher.subscribe("schedule");
    channel.bind("new-event", data => {
      this.events = [...this.events, data];
    });
  }
};
</script>

<style>
#app {
  font-family: "Work Sans", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #4a6572;
}
.main {
  display: flex;
  align-items: center;
}
.calendar-holder {
  width: 65%;
}
.form-holder {
  width: 35%;
}

.form-holder > h3 {
  color: #f9aa33;
  text-transform: uppercase;
  font-size: 16px;
  text-align: left;
  margin-left: 30px;
  margin-bottom: 10px;
}

@media (max-width: 700px) {
  .main {
    flex-direction: column;
  }
  .calendar-holder,
  .form-holder {
    width: 100%;
  }
}

::-webkit-scrollbar {
  width: 0px;
}
</style>
