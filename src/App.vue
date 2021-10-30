<template>
  <div id="app">
    <table-list v-bind="{ columns, items: users }">
      <template #header-name="{ header }">
        {{ (header.label || header.key).toUpperCase() }}
      </template>

      <template #header="{ header }">
        <div style="color:brown">{{ (header.label || header.key).toUpperCase() }}</div>
      </template>

      <template #actions="{ item: user }">
        <button style="padding: 2px 5px" @click="deleteUser(user.id)" type="button">X</button>
      </template>
    </table-list>
  </div>
</template>

<script>

import TableList from '@/components/TableList.vue';

export default {
  name: 'App',
  components: { TableList },
  data() {
    return {
      columns: [
        { key: 'name', sort: true },
        'email',
        'phone',
        'actions',
      ],
      users: [],
    };
  },
  async created() {
    await this.getUsers();
  },
  methods: {
    async getUsers() {
      const response = await fetch('https://jsonplaceholder.typicode.com/users?_limit=5');
      this.users = await response.json();
    },
    deleteUser(idx) {
      this.users = this.users.filter((item) => item.id !== idx);
    },
  },
};
</script>

<style lang="sass">
*
  margin: 0
  padding: 0
  box-sizing: border-box

#app
  max-width: 1200px
  margin: 0 auto
  padding: 50px 30px 0
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50

</style>
