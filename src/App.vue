<template>
  <div id="app">
    <table-list v-bind="{ fields, items: users }">

      <template #mainHeadField="{ field, myKey, myClass, item}">
        <div :class="myClass" :key="myKey">{{ item ? item[field] : field }}</div>
      </template>

      <template #addField="{ itemIndex, myClass }">
        <div v-if="!itemIndex" :class="myClass">Additional field</div>
        <div v-else :class="myClass">
          <button type="button" @click="deleteUser(itemIndex)">Удалить</button>
        </div>
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
      fields: ['username', 'name', 'email', 'phone'],
      users: [],
    };
  },
  created() {
    this.getUsers();
  },
  methods: {
    async getUsers() {
      const response = await fetch('https://jsonplaceholder.typicode.com/users?_limit=10');
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
