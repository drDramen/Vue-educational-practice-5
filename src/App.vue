<template>
  <div id="app">
    <test @click="sort"/>

    <table-list :items="users" :columns="columns">

      <template v-slot:title2>
        wergt
      </template>
      <template v-slot:title="{field}" >
        {{field}} ///
      </template>

<!--      <template v-slot:header-name="{header}">-->
<!--        {{ toUp(header.label) }}-->
<!--      </template>-->

<!--      <template v-slot:address="{field}">-->
<!--        str. {{ field.street }}-->
<!--      </template>-->

<!--      <template v-slot:test="{item, events}">-->
<!--        <button @click="events.edit(item)">Edit {{ item.name }}</button>-->
<!--      </template>-->

    </table-list>
    <!--    <table-list :items="test"/>-->
  </div>
</template>

<script>

import TableList from '@/components/TableList.vue';
import Test from '@/components/Test.vue';

export default {
  name: 'App',
  components: { Test, TableList },
  data() {
    return {
      users: [],
      columns: [
        // {
        //   key: 'name',
        //   label: 'Name',
        //   additional: true,
        //   editable: true,
        //   events: {
        //     blur: (e) => {
        //       console.log(e.target.innerText);
        //     },
        //     focus: () => {
        //       console.log(1);
        //     },
        //   },
        //   sort: () => this.sort('name'),
        // },
        // {
        //   key: 'test',
        //   label: 'Text',
        //   additional: true,
        //   events: {
        //     edit: (item) => {
        //       console.log(item);
        //     },
        //   },
        // },
      ],
    };
  },
  async created() {
    await this.getUsers();
  },
  methods: {
    async getUsers() {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=10');
      this.users = await response.json();
    },
    toUp(str) {
      return str.toUpperCase();
    },
    tableCellChange(e, item, fieldName) {
      const user = this.users.find((u) => u.id === item.id);
      user[fieldName] = e.target.innerText;
    },
    sort() {
      console.log(1);
    },
    // deleteUser(idx) {
    //   this.users = this.users.filter((item) => item.id !== idx);
    // },
    //   editUser(user) {
    //     this.user = user;
    //     this.edit = true;
    //   },
    //   fullyAddress({ zipcode, city, street }) {
    //     return `${zipcode}, ${city}, ${street}`;
    //   },
    //   save(e) {
    //     const form = e.target;
    //     this.fields.forEach((f) => {
    //       this.user[f] = form.elements[f].value;
    //     });
    //     this.user = {};
    //     this.edit = false;
    //   },
    //   cancel() {
    //     this.user = {};
    //     this.edit = false;
    //   },
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
