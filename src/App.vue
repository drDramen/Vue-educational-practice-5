<template>
  <div id="app">
    <form v-if="edit" class="editBlock" @submit.prevent="save">
      <template v-for="(field, idx) in fields">
        <input
          :key="idx"
          type="text"
          :name="field"
          :value="user[field]"
        >
        <!--        <input :key="idx" type="text" v-model="user[field]">-->
      </template>
      <button type="submit">Сохранить</button>
      <button @click="cancel" type="button">Отмена</button>
    </form>

    <table-list v-bind="{ fields, items: users }">

      <template #mainHeadField="{ field, myKey, myClass, item}">
        <div :key="myKey" :class="myClass">{{ item ? item[field] : field }}</div>
      </template>

      <template #addField="{ item, myClass }">
        <div v-if="!item" :class="myClass">Additional field</div>
        <div v-else :class="myClass">
          <button type="button" @click="deleteUser(item.id)">Удалить</button>
          <button type="button" @click="editUser(item)">Редактировать</button>
        </div>
        <div v-if="!item" :class="myClass">Full address</div>
        <div v-else :class="myClass">
          {{ fullyAddress(item.address) }}
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
      user: {},
      edit: false,
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
    editUser(user) {
      this.user = user;
      this.edit = true;
    },
    fullyAddress({ zipcode, city, street }) {
      return `${zipcode}, ${city}, ${street}`;
    },
    save(e) {
      const form = e.target;
      this.fields.forEach((f) => {
        this.user[f] = form.elements[f].value;
      });
      this.user = {};
      this.edit = false;
    },
    cancel() {
      this.user = {};
      this.edit = false;
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

.editBlock
  padding: 30px 20px
  text-align: left

  input, button
    display: inline-block
    margin-left: 10px
    padding: 5px 10px

</style>
