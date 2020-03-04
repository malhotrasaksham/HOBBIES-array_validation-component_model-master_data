<template>
  <div>
    <div v-for="(selected, priority) of priorities" :key="priority">
      <input
        type="checkbox"
        :disabled="selected && !currentPriorities[priority]"
        :name="itemID"
        :value="currentPriorities[priority]"
        @change="RadioChanged($event.target.checked, priority)"
        :id="itemID + priority"
      >
      <label :for="itemID + priority">{{priority}}</label>
    </div>
    <hr>
  </div>
</template>
<script>
export default {
  props: {
    value: String,
    itemID: Number,
    priorities: Object
  },
  data() {
    return {
      currentPriorities: {}
    };
  },
  created() {
    let currentPriorities = { ...this.priorities };
    Object.keys(currentPriorities).forEach(key => {
      currentPriorities[key] = false;
    });
    this.currentPriorities = currentPriorities;
  },
  methods: {
    RadioChanged(selected, priority) {
      console.log(selected, priority);
      this.currentPriorities[priority] = selected;
      this.$emit("RadioChanged", this.itemID, priority, selected);
      this.$emit("input", selected ? priority : undefined);
    }
  }
};
</script>

