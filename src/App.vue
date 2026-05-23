<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { Clock, AlarmClock, Trash2, Plus } from 'lucide-vue-next';

interface Alarm {
  id: number;
  time: string;
  label: string;
  enabled: boolean;
}

const currentTime = ref(new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' }));
const alarms = ref<Alarm[]>([]);
const newAlarmTime = ref('');

onMounted(() => {
  setInterval(() => {
    currentTime.value = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  }, 1000);
});

const addAlarm = () => {
  if (!newAlarmTime.value) return;
  alarms.value.push({
    id: Date.now(),
    time: newAlarmTime.value,
    label: 'New Alarm',
    enabled: true,
  });
  newAlarmTime.value = '';
};

const removeAlarm = (id: number) => {
  alarms.value = alarms.value.filter(a => a.id !== id);
};
</script>

<template>
  <div class="min-h-screen bg-gray-900 text-white flex flex-col items-center justify-center p-4">
    <div class="bg-gray-800 p-8 rounded-2xl shadow-xl w-full max-w-md">
      <div class="text-center mb-8">
        <h1 class="text-5xl font-mono font-bold text-blue-400">{{ currentTime }}</h1>
        <p class="text-gray-400 mt-2">Digital Alarm Clock</p>
      </div>

      <div class="space-y-4">
        <div class="flex gap-2">
          <input type="time" v-model="newAlarmTime" class="bg-gray-700 rounded-lg p-2 flex-grow outline-none border border-gray-600 focus:border-blue-500" />
          <button @click="addAlarm" class="bg-blue-600 hover:bg-blue-500 p-2 rounded-lg"><Plus /></button>
        </div>

        <ul class="space-y-2">
          <li v-for="alarm in alarms" :key="alarm.id" class="flex justify-between items-center bg-gray-700 p-3 rounded-lg">
            <span class="font-mono text-lg">{{ alarm.time }}</span>
            <button @click="removeAlarm(alarm.id)" class="text-red-400 hover:text-red-300"><Trash2 /></button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
