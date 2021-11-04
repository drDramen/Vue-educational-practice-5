<template>
  <div id="app">
    <table-list v-bind="{ sortingHeaderKey, columns, items: filteredItems }" @sort="sort">
      <template #header="{ header }">
        {{ (header.label || header.key).toUpperCase() }}
      </template>

      <!--      <template #header-name="{ header }">-->
      <!--        {{ (header.label || header.key).toUpperCase() }}-->
      <!--      </template>-->

      <template #email="{ item: user, header }">
        {{ user[header.key].toLowerCase() }}
      </template>

      <template #city="{ item: user, header }">
        {{ user.address[header.key] }}
      </template>

      <template #actions="{ item: user }">
        <button
          type="button"
          @click="deleteUser(user.id)"
        >X
        </button>
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
        { key: 'name', editable: true, sort: { direction: -1 } },
        { key: 'email', sort: { direction: 1 } },
        { key: 'phone', sort: { direction: 1 } },
        { key: 'city', additional: true, sort: { direction: 1 } },
        { key: 'actions', class: 'actions', additional: true },
      ],
      sortingHeaderKey: 'name',
      users: [],
    };
  },
  async created() {
    await this.getUsers();
  },

  computed: {
    sortingHeader() {
      return this.columns.find((h) => h.key === this.sortingHeaderKey) || {};
    },
    filteredItems() {
      let { users } = this;
      const curHeader = this.sortingHeader;
      console.log(curHeader);
      if (Object.prototype.hasOwnProperty.call(curHeader, 'sort')) {
        console.log(curHeader.sort.direction);
        const { direction } = curHeader.sort;
        users = users.sort((a, b) => {
          const fA = a[curHeader.key];
          console.log(fA);
          const fB = b[curHeader.key];
          if (fA > fB) return -1 * direction;
          if (fA < fB) return direction;
          return 0;
        });
      }
      return users;
    },
  },
  methods: {
    async getUsers() {
      const response = await fetch('https://jsonplaceholder.typicode.com/users?_limit=5');
      this.users = await response.json();
    },
    deleteUser(idx) {
      this.users = this.users.filter((item) => item.id !== idx);
    },
    sort(evt) {
      this.sortingHeaderKey = evt;
      console.log('Sort:', this.sortingHeaderKey);
      this.sortingHeader.sort.direction *= -1;
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
  //text-align: center
  color: #2c3e50

.actions
  position: relative
  padding: 0

  button
    position: absolute
    top: 0
    left: 0
    width: 100%
    height: 100%
    border: none
    background-color: #e9ce66
    cursor: pointer

    &:hover
      color: #fff

.table__body-row:last-child
  .table__body-cell:last-child.actions
    button
      border-bottom-right-radius: 10px

</style>
