<template>
  <div v-if="isEditing" class="task-list-item is-editing" @keyup.esc="cancel">
    <font-awesome-icon :icon="statusIcon" @click="$emit('toggleDone', task)" />
    <form @submit.prevent="save">
      <label>
        Title
        <input v-focus v-model.trim="editTask.title" type="text" />
      </label>
      <label>
        Due at
        <input v-model="editTask.dueAt" type="date" />
      </label>
      <span class="action-buttons">
        <button class="button isOutline" type="button" @click="cancel">cancel</button>
        <button class="button" type="submit">save</button>
      </span>
    </form>
    <font-awesome-icon :icon="deleteIcon" @click="$emit('deleteTask', task)" />
  </div>
  <div v-else class="task-list-item" >
    <font-awesome-icon :icon="statusIcon" @click="$emit('toggleDone', task)" />
    <span @click="edit">{{ task.title }}</span>
    <span @click="edit">{{ task.dueAt }}</span>
    <font-awesome-icon :icon="deleteIcon" @click="$emit('deleteTask', task)" />
  </div>
</template>

<script>
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import faCircle from '@fortawesome/fontawesome-free-regular/faCircle'
import faCheckCircle from '@fortawesome/fontawesome-free-regular/faCheckCircle'
import faTrashAlt from '@fortawesome/fontawesome-free-regular/faTrashAlt'

export default {
  components: { FontAwesomeIcon },
  directives: {
    focus: {
      inserted (el) {
        el.focus()
      }
    }
  },
  props: ['task'],
  data: () => ({
    isEditing: false,
    editTask: {}
  }),
  computed: {
    statusIcon () {
      return this.task.isComplete ? faCheckCircle : faCircle
    },
    deleteIcon () {
      return faTrashAlt
    }
  },
  methods: {
    cancel () {
      this.isEditing = false
    },
    edit () {
      this.editTask = Object.assign({}, this.task)
      this.isEditing = true
    },
    save () {
      this.$emit('updateTask', this.editTask)
      this.isEditing = false
    }
  }
}
</script>

<style lang="scss">
.task-list-item {
  display: grid;
  grid-template-columns: auto 1fr auto auto;
  grid-gap: 1rem;
  align-items: center;
  margin: 0.5rem 0;
  padding: 0.5rem;
  transition: all 0.4s;
}

.is-editing {
  margin: 1rem 0;
  background: hsl(0, 0%, 99%);
  border-radius: 0.25rem;
  box-shadow: 0px 1px 5px 0px hsl(0, 0%, 84%);
  padding-top: 1rem;
  padding-bottom: 1rem;

  form {
    display: flex;
    flex-direction: column;
  }

  label {
    display: flex;
    flex-direction: column;
    margin-bottom: 1rem;
  }

  input {
    background: hsl(0, 0%, 97%);
    border: 1px solid hsl(0, 0%, 93%);
    font-size: 1.1rem;
    padding: 0.4em;
    border-radius: 0.25rem;
  }

  .action-buttons {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 1rem;
  }

  .button {
    background: hsl(197, 90%, 43%);
    border: 1px solid hsl(197, 90%, 48%);
    border-radius: 0.25rem;
    padding: 0.5rem 0.5rem;
    color: hsl(0, 0%, 99%);
    text-transform: uppercase;
    letter-spacing: 0.5px;
    font-weight: 500;
    font-size: 1.1rem;
    box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.25);
    outline: none;

    &:active {
      box-shadow: none;
      background: hsl(197, 90%, 33%);
      border: 1px solid hsl(197, 90%, 38%);
    }

    &.isOutline {
      background: transparent;
      color: hsl(197, 90%, 43%);
      box-shadow: none;
    }
  }
}
.edit-enter,
.edit-leave-to {
  opacity: 0;
  transform: translateY(2em);
}
.edit-leave-active {
  position: absolute;
}
</style>
