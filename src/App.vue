<template>
    <div id="app">

        <nav class="bg-light py-3">
            <div class="container">
                <div class="d-flex justify-content-between align-items-center">
                    <div>
                        <h1 class="mb-0 text-uppercase">Get<span class="text-danger">Estimate</span></h1>
                        <h2 class="h6 text-dark">Быстрая оценка проекта</h2>
                    </div>
                    <div>
                        <button
                            class="btn"
                            :class="!settings.isOpen ? 'btn-outline-secondary': 'btn-secondary'"
                            title="Настройки"
                            aria-label="Настройки"
                            @click="settings.isOpen = !settings.isOpen"
                        >
                            <i class="bi bi-gear"></i>
                        </button>
                    </div>
                </div>
            </div>
        </nav>

        <div class="container py-3">
            <div class="row">
                <div class="col-xl-8">
                    <div class="pt-3">
                        <ItemForm
                            :settings="settings"
                            @create-item="createItem"
                        ></ItemForm>

                        <ItemList 
                            :settings="settings"
                            :items="items"
                            @delete-item="deleteItem"
                        ></ItemList>

                    </div>
                </div>
                <div class="col-xl-4">
                    <div class="sticky-top py-3">
                        <Settings
                            @apply-settings="applySettings"
                        ></Settings>

                        <Results
                            :settings="settings"
                            :items="items"
                            @clean-results="cleanResults"
                        ></Results>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap-icons/font/bootstrap-icons.css";


import Settings from "./components/Settings.vue";
import ItemForm from "./components/ItemForm.vue";
import ItemList from "./components/ItemList.vue";
import Results from "./components/Results.vue";
export default {
    name: "app",
    data() {
        return {
            items: [],
            settings: {},
        };
    },
    components: {
        Settings,
        ItemForm,
        ItemList,
        Results,
    },
    methods: {
        createItem(obj){
            this.items.push(obj);
        },
        deleteItem(index){
            this.items.splice(index, 1);
        },
        cleanResults(){
            this.items = [];
        },
        applySettings(settings) {
            this.settings = settings;
        }
    },
};
</script>

<style></style>
