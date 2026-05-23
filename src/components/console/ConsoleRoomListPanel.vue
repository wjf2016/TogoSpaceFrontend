<script setup lang="ts">
import { computed } from 'vue';
import { useTeamAgents, useTeamRooms } from '../../realtime/selectors';
import RoomListSection from './RoomListSection.vue';

import type { RoomState } from '../../types';

const emit = defineEmits<{
  selectRoom: [roomId: number];
  createRoom: [];
  deleteRoom: [room: RoomState];
}>();

const props = defineProps<{
  teamId: number | null;
  loading: boolean;
  currentRoomId: number | null;
}>();

const rooms = useTeamRooms(() => props.teamId);
const agents = useTeamAgents(() => props.teamId);
const createDisabled = computed(() => props.loading || !agents.value.length);
</script>

<template>
  <div class="console-panel">
    <RoomListSection
      :loading="loading"
      :rooms="rooms"
      :current-room-id="currentRoomId"
      :create-disabled="createDisabled"
      @select-room="emit('selectRoom', $event)"
      @create-room="emit('createRoom')"
      @delete-room="emit('deleteRoom', $event)"
    />
  </div>
</template>

<style scoped>
.console-panel {
  min-height: 0;
  min-width: 0;
  display: flex;
}

.console-panel > * {
  flex: 1 1 auto;
  min-height: 0;
  min-width: 0;
  height: 100%;
  width: 100%;
}
</style>
