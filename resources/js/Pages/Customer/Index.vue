<template>
    <h2 class="text-h2 mb-4">Customers</h2>
    <v-data-table
        :headers="headers"
        :items="customers"
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
                                        sm="6"
                                        md="4"
                                    >
                                        <v-text-field
                                            v-model="editedItem.email"
                                            label="Email"
                                        ></v-text-field>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="4"
                                    >
                                        <v-text-field
                                            v-model="editedItem.phone"
                                            label="Phone"
                                        ></v-text-field>
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
    {title: 'Email', key: 'email'},
    {title: 'Phone', key: 'phone'},
    {title: 'Actions', key: 'actions', sortable: false},
])
const customers = ref([])
const editedIndex = ref(-1)
const editedItem = ref({
    name: '',
    email: 0,
    phone: 0,
})
const defaultItem = ref({
    name: '',
    email: 0,
    phone: 0,

})
const formTitle = computed(() => {
    return editedIndex.value === -1 ? 'New Item' : 'Edit Item'
})

function initialize() {
    customers.value = [
        {
            name: 'Daniel',
            email: 'test1@gmail.com',
            phone: '380974513223',
        },
        {
            name: 'David',
            email: 'test2@gmail.com',
            phone: '380974513223',
        },
        {
            name: 'Bogdan',
            email: 'test3@gmail.com',
            phone: '380974513223',
        },
    ]
}

function editItem(item) {
    editedIndex.value = customers.value.indexOf(item)
    editedItem.value = Object.assign({}, item)
    dialog.value = true
}

function deleteItem(item) {
    editedIndex.value = customers.value.indexOf(item)
    editedItem.value = Object.assign({}, item)
    dialogDelete.value = true
}

function deleteItemConfirm() {
    customers.value.splice(editedIndex.value, 1)
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
        Object.assign(customers.value[editedIndex.value], editedItem.value)
    } else {
        customers.value.push(editedItem.value)
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