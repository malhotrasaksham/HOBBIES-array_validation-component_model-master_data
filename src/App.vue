<template>
  <div>
    <div v-for="(hobby, index) in hobbies" :key="index">
      <div class="formControl" :class="{'error': $v.hobbies.$each[index].hobby.$error}">
        <input
          type="text"
          @blur="$v.hobbies.$each[index].hobby.$touch()"
          placeholder="Hobby"
          v-model="hobbies[index].hobby"
        >
      </div>
      <div class="formControl" :class="{'error':$v.hobbies.$each[index].priority.$error}">
        <priority
          class="cbPriorities"
          v-model="hobbies[index].priority"
          @RadioChanged="RadioChanged"
          :priorities="priorities"
          :itemID="index"
        />
      </div>
    </div>
    <button @click="AddHobby">Add Hobby</button>
    <button :disabled="hobbies.length === 0 || $v.hobbies.$invalid" @click="Submit()">Submit</button>
  </div>
</template>
<script>
import { required } from "vuelidate/lib/validators";
import priority from "@/components/priorities";
export default {
  components: {
    priority
  },
  data() {
    return {
      priorities: {
        high: false,
        medium: false,
        low: false
      },
      hobbies: []
    };
  },
  methods: {
    AddHobby() {
      if (this.hobbies.length === 3)
        return alert("You cannot have more than 3 hobbies)");
      this.hobbies.push({
        hobby: undefined,
        priority: undefined
      });
    },
    RadioChanged(itemID, priority, selected) {
      this.priorities[priority] = selected;
      this.priorities = {
        ...this.priorities
      };
      this.$v.hobbies.$each[itemID].priority.$touch();
    },
    Submit() {
      alert("Thanks!");
    }
  },
  validations: {
    hobbies: {
      $each: {
        hobby: {
          required
        },
        priority: {
          required
        }
      }
    }
  }
};
</script>
<style scoped>
input {
  padding: 3px;
  outline: none;
  border: 1px solid #666;
}
.formControl.error input {
  border: 1px solid red;
}
.formControl.error .cbPriorities {
  background-color: #aa000011;
}
</style>

