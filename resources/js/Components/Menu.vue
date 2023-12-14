<template>
    <v-card
        width="100%"
        height="100vh"
        class="menu"

    >
        <v-list>
            <v-list-subheader>MENU</v-list-subheader>

            <Link :href="item.href" v-for="(item, i) in items">
                <v-list-item
                    :key="i"
                    :value="item"
                    color="primary"
                    rounded="shaped"
                >
                    <template v-slot:prepend>
                        <v-icon :icon="item.icon"></v-icon>
                    </template>

                    <v-list-item-title v-text="item.text"></v-list-item-title>
                </v-list-item>
            </Link>
        </v-list>
        <div class="mb-1 text-xl">
            <p class="mb-4">
                {{ user?.name }}
            </p>
            <button @click="logout"
                    class="border border-blue-100 rounded-md p-3 font-semibold w-full">
                Logout
            </button>

        </div>
    </v-card>
</template>

<script setup>
import {Link, router} from '@inertiajs/vue3'

const items = [
    {text: 'Main', icon: 'mdi-clock', href: route('admin.main.index')},
    {text: 'Customers', icon: 'mdi-account', href: route('admin.customers.index')},
    {text: 'Templates', icon: 'mdi-flag', href: route('admin.templates.index')},
]


import {usePage} from "@inertiajs/vue3";
import {computed} from "vue";

const page = usePage();

const user = computed(() => {
    return page.props.user;
})

const logout = () => {
    router.post(route('logout'));
};
</script>

<style>
.v-card__underlay {
    display: none;
}


</style>

<style scoped>
.v-card {
    padding: 16px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
</style>