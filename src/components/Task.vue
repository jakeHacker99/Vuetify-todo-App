<template>
  <div>
    <v-list-item
      @click="$store.dispatch('doneTask', task.id)"
      :class="{ 'deep-purple lighten-2': task.done }"
      class="white"
      :ripple="false"
    >
      <template v-slot:default>
        <v-list-item-action>
          <v-checkbox :input-value="task.done" color="primary"></v-checkbox>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title
            :class="{ 'text-decoration-line-through': task.done }"
            >{{ task.title }}
          </v-list-item-title>
        </v-list-item-content>
        <v-list-item-action>
          <v-list-item-action-text
          v-if="task.dueDate"
          >
          <v-icon
          small
          >mdi-calendar</v-icon>
            {{ task.dueDate | niceDate }}
          
          </v-list-item-action-text>

        </v-list-item-action>
        <v-list-item-action>

        <task-menu :task="task" />

        </v-list-item-action>

    <v-list-item-action
    v-if="$store.state.sorting"
    
    >
              <v-btn icon 
              class="handle"
              >
          <v-icon>mdi-drag-horizontal-variant</v-icon>
        </v-btn>
    </v-list-item-action>

      </template>
    </v-list-item>
    <v-divider></v-divider>
  </div>
</template>

<script>
import TaskMenu from "../components/Taskmenu";
import DialogDelete from "./Dialog/DialogDelete.vue";
import {format} from 'date-fns'

export default {
  props: ["task"],
  filters: {
    niceDate(value) {
      return format(new Date(value), 'MMM d')


    }
  },
  data() {
    return {
      dialogs: {
        delete: false,
      },
    };
  },

  components: {
    "task-menu": TaskMenu,
    "dialog-delete": DialogDelete,
    'date-fns': format
  },
};
</script>

<style lang="sass">
  .sortable-ghost
    opacity: 0
  .sortable-drag
    box-shadow: 0 0 10px rgba(0,0,0, 0.3)
</style>