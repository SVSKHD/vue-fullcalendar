<script>
import { ref } from "vue";
import "@fullcalendar/core/vdom"; // solves problem with Vite
import FullCalendar from "@fullcalendar/vue3";
import dayGridPlugin from "@fullcalendar/daygrid";
import interactionPlugin from "@fullcalendar/interaction";

export default {
  components: {
    FullCalendar, // make the <FullCalendar> tag available
  },
  setup() {
    let Events = ref([
      {id:1 , title: "event 1", date: "2022-06-01" },
      {id:2, title: "event 2", date: "2022-06-02" },
    ]);
    const handleClick = (arg) => {
      console.log("handleClick", arg);
      Events.value.push({
        title: "new event while update",
        data: "2022-06-04",
      });
      let title = prompt("Please enter a new title for your event");
      let calendarApi = arg.view.calendar;
      calendarApi.unselect(); // clear date selection
      if (title) {
        calendarApi.addEvent({
          id: 1,
          title,
          start: arg.startStr,
          end: arg.endStr,
          allDay: arg.allDay,
        });
      }
      // Events.value.push({ title: title, date:arg.startStr })
      // api data push code here
      console.log("events", Events.value, "caledar", calendarApi);
    };
    const eventClick = (arg) => {
      //Creating Data
      console.log("event", arg);
    };
    const eventUpdate = (arg) => {
      //updating Event Data
      console.log("event update" , arg.event);
    };
    return {
      //variables
      calendarOptions: {
        plugins: [dayGridPlugin, interactionPlugin],
        initialView: "dayGridMonth",
        events: Events.value,
        selectable: true,
        editable: true,
        header: {
          left: "title",
          center: "dayGridMonth, timeGridWeek, timeGridDay, listWeek",
          right: "prev today next",
        },
        //weekends:false, if weekends are false
        select: (arg) => {
          handleClick(arg);
        },
        eventClick: (arg) => {
          eventClick(arg);
        },
        eventDrop: (arg) => {
          eventUpdate(arg);
        },
      },
      Events,
      //functions
      handleClick,
    };
  },
};
</script>
<template>
  <FullCalendar
    ref="calendar"
    @select="handleClick"
    :options="calendarOptions"
  />
</template>
