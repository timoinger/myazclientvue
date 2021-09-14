<template>
    <div id="personList" >
        <p v-if="persons.length < 1" class="empty-table">
            No data
        </p>
        <Person v-else :person="person" v-for="person in persons" :key="person.id"
                @delete:person="deletePerson" @edit:person="editPerson"/>
    </div>
</template>

<script lang="ts">
    import Person from "@/components/Person";

    export default {
        name: "PersonList",
        props: {
            persons: {},
        },
        components: {
            Person,
        },
        methods: {
            deletePerson(person) {
                this.$emit('delete:person', person)
            },
            editPerson(person) {
                this.$emit('edit:person', person)
            }
        }
    }
</script>

<style scoped>

</style>
