<template>
  <div id="app">
    <table-list v-bind="{ fields, items: users }">
      <template #table-header="{ field }">
        {{ field }}
      </template>
      <template #table-body="{ item: user, myKey: key, field }">
        <div :key="key" class="table__cell">{{ user[field] }}</div>
      </template>
    </table-list>
  </div>
</template>

<script>

import TableList from '@/components/TableList.vue';

export default {
  name: 'App',
  data() {
    return {
      fields: ['username', 'name', 'email', 'phone'],
      users: [],
    };
  },
  created() {
    this.getUsers();
  },
  methods: {
    async getUsers() {
      const response = await fetch('https://jsonplaceholder.typicode.com/users?_limit=5');
      this.users = await response.json();
    },
  },
  components: { TableList },
};
</script>

<style lang="sass">
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
