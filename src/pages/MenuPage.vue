<template>
  <q-page>
    <div class="wrapper" v-if="menu && template">
      <!-- restaurant name and header image -->
      <div :class="template.styleClasses.header">
        <div class="q-pa-md">
          <div :class="template.styleClasses.restaurantName">
            {{ menu.restaurantName }}
          </div>
        </div>
        <!-- <q-img :src="menu.headerImage" class="full-width" /> -->
      </div>

      <!-- menu items -->
      <div
        v-for="category in menu.menuCategories"
        :key="category.name"
        :id="`section-${category.name}`"
      >
        <!-- category name -->
        <div
          :class="template.styleClasses.categoryName"
          class="q-mb-md q-mt-lg"
        >
          {{ category.label }}
        </div>

        <!-- items -->
        <q-list>
          <q-item
            v-for="item in menu.menuItems[category.name]"
            :key="item.itemLabel"
          >
            <q-item-section>
              <q-item-label>{{ item.itemLabel }}</q-item-label>
              <q-item-label caption v-if="item.itemDescription">
                {{ item.itemDescription }}
              </q-item-label>
            </q-item-section>

            <q-item-section side top>
              <q-item-label>{{ item.itemPrice }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </div>
    </div>

    <div v-if="menu" class="fixed-bottom-right q-pr-md q-pb-md">
      <q-fab
        v-model="fabMenu"
        vertical-actions-align="right"
        color="primary"
        icon="keyboard_arrow_up"
        direction="up"
        label="MENU"
      >
        <q-list
          bordered
          class="rounded-borders bg-white moti-border"
          style="width: 50vw"
        >
          <q-item-label header>Categories</q-item-label>

          <q-item
            clickable
            v-for="category in menu.menuCategories"
            :key="category.name"
            :href="`#section-${category.name}`"
            v-close-popup
          >
            <q-item-section>
              <q-item-label>{{ category.label }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-fab>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import { useRoute } from "vue-router";

export default defineComponent({
  name: "MenuPage",

  setup() {
    const route = useRoute();

    const restaurantId = ref(route.params.restaurantId);
    const menuId = ref(route.params.menuId);

    const menu = ref(null);
    const template = ref(null);

    fetch(`/data/menus/${restaurantId.value}/${menuId.value}.json`)
      .then((res) => res.json())
      .then((data) => {
        menu.value = data;

        fetch(`/data/templates/${menu.value.templateId}.json`)
          .then((res) => res.json())
          .then((data) => {
            template.value = data;
          });
      });

    return {
      menu,
      template,

      fabMenu: ref(true),
    };
  },
});
</script>

<style>
.moti-border {
  border: 2px solid #000;
}
</style>
