<template>
    <div 
        :class="{'d-none': !settings.isOpen}"
        class="card bg-light mb-4"
    >
        <div class="card-header">
            <h4 class="card-title h5 mb-1">Настройки:</h4>
        </div>
        <div class="card-body">
            <form>
                <div>
                    <h5 class="h6">Единица измерения:</h5>
                    <div class="form-check">
                        <input
                            id="settingsUnit-0"
                            class="form-check-input"
                            name="unit"
                            type="radio"
                            :value=0
                            v-model="settings.unit"
                        />
                        <label 
                            class="form-check-label"
                            for="settingsUnit-0"
                        >
                            В часах
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            id="settingsUnit-1"
                            class="form-check-input"
                            name="unit"
                            type="radio"
                            :value=1
                            v-model="settings.unit"
                        />
                        <label 
                            class="form-check-label"
                            for="settingsUnit-1"
                        >
                            В рублях
                        </label>
                    </div>
                </div>
                <hr class="hr">
                <div>
                    <h5 class="h6">Вилка оценки:</h5>
                    <div class="form-check">
                        <input
                            id="settingsFork-0"
                            class="form-check-input"
                            name="fork"
                            type="radio"
                            :value=0
                            v-model="settings.fork"
                        />
                        <label 
                            class="form-check-label"
                            for="settingsFork-0"
                        >
                            Одно значение
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            id="settingsFork-1"
                            class="form-check-input"
                            name="fork"
                            type="radio"
                            :value=1
                            v-model="settings.fork"
                        />
                        <label 
                            class="form-check-label"
                            for="settingsFork-1"
                        >
                            Два значения (от, до)
                        </label>
                    </div>
                </div>
                <hr class="hr">
                <div>
                    <h5 class="h6">Нумерация пунктов в результате:</h5>
                    <div class="form-check">
                        <input
                            id="settingsNumbers-0"
                            class="form-check-input"
                            name="numbers"
                            type="radio"
                            :value=1
                            v-model="settings.numbers"
                        />
                        <label 
                            class="form-check-label"
                            for="settingsNumbers-0"
                        >
                            Включить
                        </label>
                    </div>
                    <div class="form-check">
                        <input
                            id="settingsNumbers-1"
                            class="form-check-input"
                            name="numbers"
                            type="radio"
                            :value=0
                            v-model="settings.numbers"
                        />
                        <label 
                            class="form-check-label"
                            for="settingsNumbers-1"
                        >
                            Отключить
                        </label>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>

<script>
export default { 
    //   name: 'Results',
    data() {
        return {
            settings: {
                unit: 0,
                fork: 1,
                numbers: 1,
                isOpen: false,
            },
        };
    },
    created: function () {
        this.$emit('apply-settings', this.settings);
    },
    methods: {
        // applySettings(settings) {
        //     this.$emit('apply-settings', settings);
        // }
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .form-check-input:checked {
        background-color: #6c757d;
        border-color: #6c757d;
    }
    .form-check-input:focus {
        box-shadow: 0 0 0 0.25rem rgb(108 117 125 / 50%);
    }
</style>
