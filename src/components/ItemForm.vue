<template>
    <div class="card bg-light mb-5">
        <div class="card-body">
            <div class="row">
                <div class="col-lg-9 pe-auto pe-lg-1">
                    <h3 class="mb-4 text-dark">
                        Добавить новый пункт к оценке
                    </h3>
                </div>
                <div class="col-lg-3 ps-auto ps-lg-1 text-end">
                    <button
                        class="btn"
                        :class="!importByPlainTextMode ? 'btn-outline-secondary': 'btn-secondary'"
                        title="Импорт из списка"
                        aria-label="Импорт из списка"
                        @click="importByPlainTextMode = !importByPlainTextMode"
                    >
                        <i class="bi bi-list-ul"></i>
                    </button>
                </div>
            </div>

            <template v-if="importByPlainTextMode == false">
                <form @submit.prevent="createItem">
                    <div class="row">
                        <div class="col-lg-5 pe-auto pe-lg-1">
                            <div class="mb-2">
                                <label class="form-label text-muted" for="title">Название</label>
                                <input
                                    type="text"
                                    id="title"
                                    v-model="title"
                                    class="form-control w-100"
                                    :class="{ 'is-invalid': !isValid }"
                                    placeholder=""
                                />
                            </div>
                        </div>
                        <div class="col-lg-4 px-auto px-lg-1">
                            <div class="d-flex">
                                <template v-if="settings.fork">
                                    <div class="flex-grow-1 w-50 pe-1">
                                        <div class="mb-2">
                                            <label class="form-label text-muted" for="from">
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
                                                id="from"
                                                v-model="from"
                                                class="form-control w-100"
                                                min="0"
                                                @focus="$event.target.select()"
                                            />
                                        </div>
                                    </div>
                                    <div class="flex-grow-1 w-50 ps-1">
                                        <div class="mb-2">
                                            <label class="form-label text-muted" for="to">
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
                                                id="to"
                                                v-model="to"
                                                class="form-control w-100"
                                                min="0"
                                                @focus="$event.target.select()"
                                            />
                                        </div>
                                    </div>
                                </template>
                                <template v-else>
                                    <div class="flex-grow-1 px-lg-1">
                                        <div class="mb-2">
                                            <label class="form-label" for="from">
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
                                                id="from"
                                                v-model="from"
                                                class="form-control w-100"
                                                min="0"
                                                @focus="$event.target.select()"
                                            />
                                        </div>
                                    </div>
                                </template>
                            </div>
                        </div>
                        <div class="col-lg-3 ps-auto ps-lg-1 align-self-end">
                            <div class="mb-2">
                                <button type="submit" class="btn btn-dark w-100 text-nowrap" >
                                    добавить
                                </button>
                            </div>
                        </div>
                    </div>
                    <div class="mb-2">
                        <a
                            class="text-danger"
                            href="javascript:void(0)"
                            @click="showDescription = !showDescription"
                        >
                            <template v-if="showDescription">
                                не добавлять описание
                            </template>
                            <template v-else> добавить описание </template>
                        </a>
                    </div>
                    <div :class="{ 'd-none': !showDescription }">
                        <div>
                            <label class="form-label" for="description">Описание</label>
                            <textarea
                                name="description"
                                id="description"
                                v-model="description"
                                rows="6"
                                class="form-control"
                            ></textarea>
                        </div>
                    </div>
                </form>

            </template>
            <template v-else>
                <form @submit.prevent="importByPlainText">
                    <div class="mb-2">
                        <label class="form-label" for="plainText">Импортировать пункты оценки из списка</label>
                        <textarea
                            id="plainText"
                            v-model="plainText"
                            class="form-control"
                            cols="30"
                            rows="10"
                        ></textarea>
                        <div class="small text-muted mb-2">
                            Вставьте пункты оценки простым текстовым списком. Каждая
                            новая строка - новый пункт оценки.
                        </div>
                    </div>
                    <button class="btn btn-dark">
                        Импорт
                    </button>
                </form>
            </template>
        </div>
    </div>
</template>

<script>
export default {
    //   name: 'ItemForm',
    data() {
        return {
            title: '',
            from: 0,
            to: 0,
            description: '',
            isValid: true,
            showDescription: false,
            plainText: '',
            importByPlainTextMode: false,
        };
    },
    props: ['settings'],
    emits: ['createItem', 'create-item', 'onCreate-item'], 
    /*
    list of emits should be just  "emits: ['createItem']"
    the same case is here https://stackoverflow.com/questions/64947719/vue-3-emit-warning-even-though-emits-is-present
    */
    methods: {
        createItem: function (){
            let title = this.title;
            let from = this.from;
            let to = this.to;
            let description = this.description;

            let descriptionValue = '';
            let descriptionShow = false;

            if (this['showDescription'] && description) {
                descriptionValue = description;
                descriptionShow = true; 
            }

            let obj = { 
                'title': title,
                'from': (from)?from:0,
                'to': (to)?to:0,
                'description': descriptionValue,
                'isValid': true,
                'showDescription': descriptionShow,
                'isExclude': false,
            };
            if (obj.title) {
                this.$emit('create-item', obj);
                this['isValid'] = true;
                this['title'] = "";
                this['from'] = 0;
                this['to'] = 0;
                this['description'] = "";
            } else {
                this['isValid'] = false;
            }
            document.getElementById('title').focus();
        },

        importByPlainText: function(){
            let plainText = this.plainText;
            let array = plainText.split(/\r?\n/);
            for (let index = 0; index < array.length; index++) {
                array[index] = array[index].trim();
                if (array[index] != '') {
                    let obj = { 
                        'title': array[index],
                        'from': 0,
                        'to': 0,
                        'description': '',
                        'isValid': true,
                        'showDescription': false,
                        'isExclude': false,
                    };
                    this.$emit('create-item', obj);
                }
            }
            this.plainText = '';
            this['importByPlainTextMode'] = false;
        },
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
