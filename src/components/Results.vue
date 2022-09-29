<template>
    <div class="card bg-light">
        <template v-if="items.length > 0">
            <div class="card-header">
                <div class="mb-2">
                    <strong class="text-muted">
                        Итого по оценке:
                        <template v-if="settings.fork">
                            {{ sum("from") }} - {{ sum("to") }}
                        </template>
                        <template v-else>
                            {{ sum("from") }}
                        </template>

                        <template v-if="settings.unit">
                            <span class="text-muted"> рублей</span>
                        </template>
                        <template v-else>
                            <span class="text-muted"> часов</span>
                        </template>
                    </strong>
                </div>

                <div class="d-flex">
                    <button class="btn btn-dark me-1" @click="copyResult(settings)">
                        Копировать
                    </button>
                    <button class="btn btn-danger ms-1" @click="deleteResult">
                        Очистить
                    </button>
                </div>
                <div
                    class="text-muted"
                    :class="copyResultMessage == '' ? '' : 'mt-2'"
                >
                    {{ copyResultMessage }}
                </div>
            </div>
        </template>
        <div class="card-body">
            <h4 class="card-title h6">Результат:</h4>
            <template v-if="items.length > 0">
                <ol
                    class="mb-0"
                    :class="settings.numbers ? 'ps-3' : 'list-unstyled'"
                >

                    <draggable 
                        :list="items"
                        @start="drag=true" 
                        @end="drag=false" 
                        item-key="id"
                    >
                        <template #item="{element}">
                            <li class="mb-2">
                                <div
                                    :class="element.isExclude ? 'text-decoration-line-through' : ''"
                                >
                                    <div>{{ element.title }}</div>
                                    <div
                                        class="text-muted"
                                        v-if="element.showDescription"
                                    >
                                        {{ element.description }}
                                    </div>
                                    <div>
                                        <template v-if="settings.fork">
                                            {{ element.from }} - {{ element.to }}
                                        </template>
                                        <template v-else>
                                            {{ element.from }}
                                        </template>

                                        <template v-if="settings.unit">
                                            <span class="text-muted"> рублей</span>
                                        </template>
                                        <template v-else>
                                            <span class="text-muted"> часов</span>
                                        </template>
                                    </div>
                                </div>
                            </li>
                        </template>
                    </draggable>
                </ol>
            </template>
            <template v-else>
                <span class="text-muted"> Пока ничего нет </span>
            </template>
        </div>
    </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
    //   name: 'Results',
    data() {
        return {
            copyResultMessage: '',
            drag: false,
        };
    },
    props: ['items', 'settings'],
    components: {
        draggable,
    },
    methods: {
        sum: function (prop) {
            let total = 0;
            for (let i = 0, _len = this['items'].length; i < _len; i++) {
                if (this['items'][i]['isExclude'] === false) {
                    let value = this['items'][i][prop];
                    if(!value){
                        value = 0;
                    }
                    total += parseFloat(value);
                }
            }
            return total;
        },

        copyResult: function (settings) {
            let unit = settings.unit ? 'рублей' : 'часов';

            let result = '';
            let number = 0;
            for (let index = 0; index < this['items'].length; index++) {
                const element = this['items'][index];
                if (!element['isExclude']) {

                    if (settings.numbers){
                        number++;
                        result += number + '. ';
                    }

                    result += element['title'] + '\n';
                    
                    if (element['description']) {
                        result += 'Описание: ' + element['description'] + '\n';
                    }

                    if (settings.fork){
                        result += element['from'] + ' - ' + element['to'] + ' ' + unit + ' \n';
                    } else {
                        result += element['from'] + ' ' + unit + ' \n';
                    }
                    result += '\n';
                }
            }
            result += '\n';

            if (settings.fork){
                result += 'Итого по оценке: ' + this.sum('from') + ' - ' + this.sum('to') + ' ' + unit;
            } else {
                result += 'Итого по оценке: ' + this.sum('from') + ' ' + unit;
            }

            result += '\n';
            result += '\n';
            result += '\n';

            result += '__________________________________________________________\n';
            result += 'Оценка составлена с помощью проекта GetEstimate\n';
            result += 'https://dimadroog.github.io/estimate/';

            navigator.clipboard.writeText(result);

            this['copyResultMessage'] = 'Результат скопирован в буфер обмена';
            setTimeout(
                function (scope) {
                    scope['copyResultMessage'] = '';
                },
                3000,
                this
            );
        },

        deleteResult: function () {
            if (confirm('Действительно хотите очистить данные?')) {
                this.$emit('clean-results');
            }
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ol li {
    cursor: move;
}
</style>
