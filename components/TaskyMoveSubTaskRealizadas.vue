<template>
  <div class="mt-5 mb-5">
    <v-menu offset-y>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="#00796B"
          dark
          v-bind="attrs"
          v-on="on"
        >
          MOVER A
        </v-btn>
      </template>
      <v-list color="#80CBC4">
        <v-list-item
          v-for="(item, index) in items"
          :key="index"
        >
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </div>
</template>

<script>
  export default {
    data: () => ({
      items: [
        { title: 'Tareas' },
        { title: 'En Curso' },
      ],
    }),
  }
</script>