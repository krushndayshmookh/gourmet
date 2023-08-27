<template>
  <q-layout view="lHh Lpr lFf">
    <q-header bordered class="bg-white">
      <q-toolbar>
        <q-toolbar-title class="text-amber-9 text-center text-uppercase text-barlow text-body1"> Gourmet </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "EmptyLayout",

  setup() {
    return {};
  },
});
</script>

