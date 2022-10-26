<script lang="ts">
import { defineComponent } from 'vue';
import MyForm from './MyForm.vue';

export default defineComponent({
  components: { MyForm },
});
</script>

<template>
  <v-app>
    <v-main>
      <v-container class='align-center justify-center fill-height'>
        <v-row class='align-center justify-center fill-height'>
          <v-col md=5 cols=12>
            <MyForm :initialUser='{ fullName: "Bob", email: "bob@email.com" }'></MyForm>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

