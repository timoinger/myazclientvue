<template>
    <div id="person">
        <table class="tableStyle">
            <tr>
                <td>
                    <GiphyImage :name="person.name" />
                </td>

                <td v-if="editing === person.id">
                    <input type="text" v-model="person.name" />
                </td>
                <td v-else>{{person.name}}</td>

                <td v-if="editing === person.id">
                    <button @click="editPerson(person)">Save</button>
                    <button @click="editing = null">Cancel</button>
                </td>
                <td v-else>
                    <button @click="editMode(person)">Edit</button>
                    <button @click="$emit('delete:person', person)">Delete</button>
                </td>
            </tr>
        </table>
    </div>
</template>

<script lang="ts">
    import GiphyImage from "./GiphyImage.vue";

    export default {
        name: "Person",
        components: {GiphyImage},
        props: {
            person: {}
        },
        data() {
            return {
                editing: null,
            }
        },
        methods: {
            editMode(person) {
              this.editing = person.id
            },
            editPerson(person) {
                if (person.name === '') return
                this.$emit('edit:person', person)
                this.editing = null
            }
        }
    }
</script>

<style scoped>
    .tableStyle {
        display: inline-block;
    }
</style>
