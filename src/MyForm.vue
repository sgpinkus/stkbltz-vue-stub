<script lang='ts'>
import { defineComponent, toRaw } from 'vue';
import { isEmpty } from 'lodash';

function COW(base: object, updates: object) {
  return new Proxy(base, {
    get(target: object, key: any): unknown {
      if (key in updates)
        return Reflect.get(updates, key);
      return Reflect.get(target, key);
    },
    set(target: object, key: any, value: any): boolean {
      return Reflect.set(updates, key, value);
    },
  });
}

export default defineComponent({
  props: {
    initialUser: {
      required: true,
      type: Object,
    },
  },
  data(): any {
    const updates = {};
    return {
      user: COW(this.initialUser, updates),
      updates,
      valid: true,
    };
  },
  computed: {
    changed() {
      console.log('computing change');
      return !isEmpty(this.updates);
    },
  },
  watch: {
    updates: {
      handler() {
        console.log('updates changed');
      },
      deep: true,
    },
  },
  methods: {
    reset () {
      (this.$refs.form as any).resetValidation();
      this.updates = {};
      (this.$refs.form as any).validate();
    },
    submit() {
      console.debug(toRaw(this.updates), isEmpty(this.updates));
    },
  }
});
</script>

<template>
  <v-form v-if='user' ref="form" v-model="valid">
    <v-text-field
      v-model="user.fullName"
      counter="32"
      label="Full Name"
    ></v-text-field>
    <v-text-field
      v-model="user.email"
      counter="64"
      type="email"
      label="Email"
    ></v-text-field>
    <div class="d-flex justify-end align-baseline">
      <v-btn color="error" class="mr-4" @click="reset">Reset</v-btn>
      <v-btn :disabled="!valid || !changed" color="warning" @click="submit">Submit</v-btn>
    </div>
  </v-form>
</template>