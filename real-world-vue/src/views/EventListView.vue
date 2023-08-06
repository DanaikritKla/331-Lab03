<script setup lang="ts">
import EventCard from '../components/EventCard.vue'
import EventCard2 from '../components/EventCard2.vue'
import type { EventItem } from '@/type';
import { ref, watchEffect, type Ref } from 'vue';
import EventService from '@/services/EventService'
import type { Axios, AxiosResponse } from 'axios';

const events: Ref<EventItem[]> = ref([])
  const props = defineProps({
    page: {
      type: Number,
      required: true
    }
  })

  EventService.getEvent(2, props.page).then((response: AxiosResponse<EventItem[]>) => {
    events.value = response.data
  })

  watchEffect(() => {
    EventService.getEvent(2, props.page).then((response: AxiosResponse<EventItem[]>) => {
      events.value = response.data
    })
  })
  
</script>

<template>
  <h1>Events For Good</h1>
  <main class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event"></EventCard>
    <RouterLink :to="{ name: 'event-list', query: { page: page - 1 } }" rel="prev" v-if="page != 1">
      Prev Page
    </RouterLink>
    <RouterLink :to="{ name: 'event-list', query: { page: page + 1 } }" rel="next" v-if="page + 1">
      Next Page
    </RouterLink>
  </main>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.events2 {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: right;
}
</style>
