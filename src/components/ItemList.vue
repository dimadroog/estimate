<template>
    <ul v-if="items.length > 0" :class="{'drag': settings.drag == 1}" class="p-0">
        <draggable 
            :disabled='settings.drag == 0'
            :list="items"
            @start="drag=true" 
            @end="drag=false" 
            item-key="id"
        >
            <template #item="{element, index}">
                <li 
                    class="card mb-3" 
                    :class="{'bg-light': element.isExclude}"
                    id="index"
                >
                    <div class="card-body">
                        <div class="row">
                            <div class="col-lg-5 pe-auto pe-lg-1">
                                <div class="mb-2">
                                    <label class="form-label text-muted" :for="'title-' + index">Название</label>
                                    <input 
                                        type="text" 
                                        :id="'title-' + index"
                                        class="form-control w-100"
                                        v-model="element.title" 
                                        :disabled="element.isExclude == true"
                                        placeholder=""
                                    >
                                </div>
                            </div>
                            <div class="col-lg-3 px-auto px-lg-1">
                                <div class="d-flex">
                                    <template v-if="settings.fork">
                                    <div class="flex-grow-1 w-50 pe-1">
                                        <div class="mb-2">
                                            <label class="form-label text-muted" :for="'from-' + index">
                                                от,
                                                <template v-if="settings.unit">
                                                    рублей
                                                </template>
                                                <template v-else>
                                                    часов
                                                </template>
                                            </label>
                                            <input 
                                                type="number" 
                                                :id="'from-' + index" 
                                                class="form-control w-100" 
                                                v-model="element.from" 
                                                :disabled="element.isExclude == true"
                                                min="0"
                                                @focus="$event.target.select()"
                                                @change="isEmpty(element, 'from')"
                                            >
                                        </div>
                                    </div>
                                    <div class="flex-grow-1 w-50 ps-1">
                                        <div class="mb-2">
                                            <label class="form-label text-muted" :for="'to-' + index">
                                                до,
                                                <template v-if="settings.unit">
                                                    рублей
                                                </template>
                                                <template v-else>
                                                    часов
                                                </template>
                                            </label>
                                            <input 
                                                type="number" 
                                                :id="'to-' + index" 
                                                v-model="element.to"
                                                class="form-control w-100" 
                                                :disabled="element.isExclude == true"
                                                min="0"
                                                @focus="$event.target.select()"
                                                @change="isEmpty(element, 'to')"
                                            >
                                        </div>
                                    </div>
                                    </template>
                                    <template v-else>
                                        <div class="flex-grow-1 px-lg-1">
                                            <div class="mb-2">
                                                <label class="form-label" :for="'from-' + index">
                                                    оценка,
                                                    <template v-if="settings.unit">
                                                        <span class="text-muted">рублей</span>
                                                    </template>
                                                    <template v-else>
                                                        <span class="text-muted">часов</span>
                                                    </template>
                                                </label>
                                                <input 
                                                    type="number" 
                                                    :id="'from-' + index" 
                                                    class="form-control w-100" 
                                                    v-model="element.from" 
                                                    :disabled="element.isExclude == true"
                                                    min="0"
                                                    @focus="$event.target.select()"
                                                    @change="isEmpty(element, 'from')"
                                                >
                                            </div>
                                        </div>
                                    </template>
                                </div>
                            </div>
                            <div class="col-lg-4 ps-auto ps-lg-1 align-self-end">
                                <div class="mb-2">
                                    <div class="d-flex">
                                        <div class="flex-grow-1 w-50 pe-1">
                                            <button 
                                                @click="element.isExclude = !element.isExclude"
                                                class="btn btn-secondary w-100 px-1 text-nowrap"
                                            >
                                                <template v-if="element.isExclude" >
                                                    учитывать
                                                </template>
                                                <template v-else>
                                                    не учитывать
                                                </template>
                                            </button>
                                        </div>
                                        <div class="flex-grow-1 w-50 ps-1">
                                            <button 
                                                @click="deleteItem(index)"
                                                :disabled="element.isExclude == true"
                                                class="btn btn-danger w-100 text-nowrap"
                                            >удалить</button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div>
                            <div class="mb-2">
                                <a 
                                    class="text-danger" 
                                    href="javascript:void(0)"
                                    @click="element.showDescription = !element.showDescription"
                                >
                                    <template v-if="element.showDescription" >
                                        скрыть описание
                                    </template>
                                    <template v-else>
                                        добавить описание
                                    </template>
                                </a>
                            </div>
                            <div :class="{'d-none': !element.showDescription}">
                                <label class="form-label" :for="'description-' + index">Описание</label>
                                <textarea 
                                    :id="'description-' + index" 
                                    rows="6" 
                                    class="form-control"
                                    v-model="element.description"
                                ></textarea>
                            </div>
                        </div>
                    </div>
                </li>
            </template>
        </draggable>
    </ul>
</template>

<script>
import draggable from "vuedraggable";

export default {
    //   name: 'ItemList',
    props: [
        'items',
        'settings',
    ],
    emits: ['deleteItem', 'delete-item', 'onDelete-item'], 
    data() {
        return {
            drag: false,
        };
    },
    components: {
        draggable,
    },
    methods: {
        deleteItem(index) {
            this.$emit("delete-item", index);
        },
        isEmpty(element, prop) {
            if (element[prop] === ''){
                element[prop] = 0;
            }
        }
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    ul.drag li.card {
        cursor: move;
    }
</style>
