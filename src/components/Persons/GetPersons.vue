<template src="./template.html"></template>
<style src="./style.css"></style>

<script>
const axios = require('axios');
export default {
  name: "GetPersons",
  data() {
    return {
      data: [],
      group: [],
      enteredText: '',
      sortType: 'default',
      previewType: 'preview',
      sortOptions: [
        { text: 'Default sort', value: 'default' },
        { text: 'A-Z', value: 'fullName' },
        { text: 'Balance', value: 'balance' },
        { text: 'isActive', value: 'isActive' },
        { text: 'Registered', value: 'registered' },
        { text: 'State', value: 'state' },
        { text: 'Country', value: 'country' }
      ],
      showItems: [
        {text: 'Show items per page', value: 'preview'},
        {text: '20', value: '20'},
        {text: '50', value: '50'},
        {text: '100', value: '100'},
      ]
    }
  },
  computed: {
    filteredPerson() {
      let result = this.group;

      // early return
      if (!this.enteredText)
        return result;

      const filterValue = this.enteredText.toLowerCase()

      const filter = event =>
          event.fullName.toLowerCase().includes(filterValue) ||
          event.balance.toLowerCase().includes(filterValue) ||
          event.isActive.toString().toLowerCase().includes(filterValue) ||
          event.registered.toLowerCase().includes(filterValue) ||
          event.state.toLowerCase().includes(filterValue) ||
          event.country.toLowerCase().includes(filterValue);

      return result.filter(filter)
    }
  },
  methods: {
    async getUser() {
      try {
        const response = await axios.get("https://fww-demo.herokuapp.com/");
        let helper = [];
        this.data = response.data;
        this.data.forEach(st => {
          st.state.forEach(us => {
            let aboutUser = us.users.map(about => {
              return {
                fullName: about.fullName,
                balance: about.balance,
                isActive: about.isActive,
                registered: about.registered,
                state: us.name,
                country: st.country,
              }
            })
            Array.prototype.push.apply(helper, aboutUser);
          })
        })
        this.group = helper;
      } catch (error) {
        console.error(error);
      }
    },
    sortBy(sortKey) {
      this.group.sort((a, b) =>
          (typeof a[sortKey] === 'string' || typeof b[sortKey] === 'string') ?
              a[sortKey].localeCompare(b[sortKey]) : a[sortKey] - b[sortKey]);
    },
  },
  mounted() {
    this.getUser();
  },
}
</script>