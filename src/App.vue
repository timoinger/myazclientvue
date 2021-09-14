<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">

        <HelloWorld msg="Vue"/>
        <PersonForm @add:person="addPerson" />
        <PersonList :persons=persons @delete:person="deletePerson" @edit:person="editPerson" />
  </div>
</template>

<script lang="ts">
  import HelloWorld from './components/HelloWorld.vue'
  import PersonForm from './components/PersonForm.vue'
  import PersonList from './components/PersonList.vue'

//  const URL = process.env.VUE_APP_BACKEND_URL || 'set VUE_APP_BACKEND_URL in .env files!';
  const URL = 'https://myazserverspring.azurewebsites.net'
  console.log('VUE_APP_BACKEND_URL: ', URL);

  export default {
  name: 'App',
  components: {
    HelloWorld,
    PersonForm,
    PersonList,
  },
  data() {
    return {
        persons: []
    }
  },
  mounted() {
      this.getAllPersons()
  },

  methods: {
    async getAllPersons() {
        try {
            const requestOptions = {
                method: 'GET',
                headers: {'Content-Type': 'application/json'}
            };
            const response = await fetch(URL, requestOptions)
            this.persons = await response.json()
            this.$forceUpdate();
        } catch (error) {
            console.error(error)
        }
    },

    async addPerson(name) {
      try {
          const requestOptions = {
              method: 'POST',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({'name': name}),
          };

          await fetch(URL, requestOptions)

          await this.getAllPersons()
      } catch (error) {
          console.error(error)
      }
    },

    async deletePerson(person) {
      try {
          const requestOptions = {
              method: 'DELETE',
              headers: {'Content-Type': 'application/json'},
          };

          await fetch(URL + '/' + person.id, requestOptions);
          this.persons = this.persons.filter(p => person.id !== p.id);
      } catch (error) {
          console.error(error);
      }
    },
      async editPerson(person) {
          try {
              const requestOptions = {
                  method: 'PUT',
                  headers: {'Content-Type': 'application/json'},
                  body: JSON.stringify({name: person.name}),
              };
              await fetch(URL + '/' + person.id, requestOptions)
          } catch (error) {
              console.error(error)
          }
      }
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
