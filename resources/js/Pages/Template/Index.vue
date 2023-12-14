<template>
    <h2 class="text-h2 mb-4">Templates</h2>
    <v-data-table
        :headers="headers"
        :items="templates"
    >
        <template v-slot:top>
            <v-toolbar
                flat
            >
                <v-divider
                    class="mx-4"
                    inset
                    vertical
                ></v-divider>
                <v-spacer></v-spacer>
                <v-dialog
                    v-model="dialog"
                    max-width="500px"
                >
                    <template v-slot:activator="{ props }">
                        <v-btn
                            color="primary"
                            dark
                            class="mb-2"
                            v-bind="props"
                        >
                            New Item
                        </v-btn>
                    </template>
                    <v-card>
                        <v-card-title>
                            <span class="text-h5">{{ formTitle }}</span>
                        </v-card-title>

                        <v-card-text>
                            <v-container>
                                <v-row>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="4"
                                    >
                                        <v-text-field
                                            v-model="editedItem.name"
                                            label="Name"
                                        ></v-text-field>
                                    </v-col>
                                    <v-col
                                        cols="12"

                                    >
                                        <v-textarea
                                            v-model="editedItem.content"
                                            label="Content"
                                        ></v-textarea>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="4"
                                    >

                                        <v-checkbox
                                            v-model="editedItem.is_special"
                                            label="Is Special"

                                        >
                                        </v-checkbox>
                                    </v-col>
                                </v-row>
                            </v-container>
                        </v-card-text>

                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn
                                color="blue-darken-1"
                                variant="text"
                                @click="close"
                            >
                                Cancel
                            </v-btn>
                            <v-btn
                                color="blue-darken-1"
                                variant="text"
                                @click="save"
                            >
                                Save
                            </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
                <v-dialog v-model="dialogDelete" max-width="500px">
                    <v-card>
                        <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
                            <v-btn color="blue-darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
                            <v-spacer></v-spacer>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </v-toolbar>
        </template>
        <template v-slot:item.is_special="{ item }">
            <v-checkbox
                v-model="item.is_special"
                readonly
            ></v-checkbox>
        </template>
        <template #item.actions="{ item }">
            <v-icon
                size="small"
                class="me-2"
                @click="editItem(item)"
            >
                mdi-pencil
            </v-icon>
            <v-icon
                size="small"
                @click="deleteItem(item)"
            >
                mdi-delete
            </v-icon>
        </template>
        <template v-slot:no-data>
            <v-btn
                color="primary"
                @click="initialize"
            >
                Reset
            </v-btn>
        </template>
    </v-data-table>
</template>

<script setup>
import {computed, nextTick, ref, watch} from 'vue'

const dialog = ref(false)
const dialogDelete = ref(false)
const headers = ref([
    {title: 'Name', key: 'name'},
    {title: 'Content', key: 'content'},
    {title: 'Is Special', key: 'is_special'},
    {title: 'Actions', key: 'actions', sortable: false},
])
const templates = ref([])
const editedIndex = ref(-1)
const editedItem = ref({
    name: '',
    content: '',
    is_special: false,
})
const defaultItem = ref({
    name: '',
    content: '',
    is_special: false,

})
const formTitle = computed(() => {
    return editedIndex.value === -1 ? 'New Item' : 'Edit Item'
})

function initialize() {
    templates.value = [
        {
            name: 'Template #1',
            content: `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut
                 labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi
                 ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
                 cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
                 culpa qui officia deserunt mollit anim id est laborum.`,
            is_special: true,
        },
        {
            name: 'Template #2',
            content: `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut
                 labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi
                 ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
                 cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
                 culpa qui officia deserunt mollit anim id est laborum.`,
            is_special: true,
        },
        {
            name: 'Template #3',
            content: `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut
                 labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi
                 ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
                 cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
                 culpa qui officia deserunt mollit anim id est laborum.`,
            is_special: false,
        },
    ]
}

function editItem(item) {
    editedIndex.value = templates.value.indexOf(item)
    editedItem.value = Object.assign({}, item)
    dialog.value = true
}

function deleteItem(item) {
    editedIndex.value = templates.value.indexOf(item)
    editedItem.value = Object.assign({}, item)
    dialogDelete.value = true
}

function deleteItemConfirm() {
    templates.value.splice(editedIndex.value, 1)
    closeDelete()
}

function close() {
    dialog.value = false
    nextTick(() => {
        editedItem.value = Object.assign({}, defaultItem.value)
        editedIndex.value = -1
    })
}

function closeDelete() {
    dialogDelete.value = false
    nextTick(() => {
        editedItem.value = Object.assign({}, defaultItem.value)
        editedIndex.value = -1
    })
}

function save() {
    if (editedIndex.value > -1) {
        Object.assign(templates.value[editedIndex.value], editedItem.value)
    } else {
        templates.value.push(editedItem.value)
    }
    close()
}

watch(dialog, val => {
    val || close()
})
watch(dialogDelete, val => {
    val || closeDelete()
})
initialize()
</script>

