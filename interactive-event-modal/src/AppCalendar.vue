<script setup>
import { ScheduleXCalendar } from '@schedule-x/vue'
import {
  createCalendar,
  createViewDay,
  createViewMonthGrid,
  createViewWeek,
} from '@schedule-x/calendar'
import '@schedule-x/theme-default/dist/index.css'
import '@sx-premium/interactive-event-modal/index.css'
import {shallowRef} from "vue";
import {eventsService} from "./plugins/events-service.ts";
import {eventModal} from "./plugins/modal.ts";

// Important. Use shallowRef instead of ref, since ref makes all child properties reactive, which causes errors in the calendar.
const calendarApp = shallowRef(createCalendar({
  selectedDate: '2025-03-01',
  views: [
    createViewDay(),
    createViewWeek(),
    createViewMonthGrid(),
  ],
  plugins: [
    eventsService,
    eventModal,
  ],
  callbacks: {
    onDoubleClickDateTime: (dateTime) => {
      eventModal.clickToCreate(dateTime)
    },
  },
  events: [
    {
      id: 1,
      title: 'Event 1',
      start: '2025-03-01',
      end: '2025-03-01',
      customAtt: "CustomAtt"
    },
    {
      id: 2,
      title: 'Event 2',
      start: '2025-03-02 12:00',
      end: '2025-03-02 13:00',
      customAtt: "CustomAtt"
    },
  ],
}))
</script>

<template>
  <div>
    <ScheduleXCalendar :calendar-app="calendarApp" >
      <template #interactiveModalAdditionalFields="{ calendarEvent }">
        This should be displayed when clicking on an Event, this is fully functional with the basic eventModal
        <button @click="console.log(calendarEvent)" style="color: red"> CLICK 4 INFO </button>
        {{ calendarEvent.customAtt }}
      </template>
    </ScheduleXCalendar>
  </div>
</template>
