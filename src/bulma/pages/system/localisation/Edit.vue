<template>
    <div class="columns is-centered">
        <div class="column is-three-quarters-desktop is-full-touch">
            <enso-form class="box has-background-light raises-on-hover animated fadeIn">
                <template v-slot:flag="{ field }">
                    <div class="control has-icons-right">
                        <input class="input"
                            :value="field.value"
                            type="text"
                            readonly="readonly">
                        <span class="icon is-small is-right">
                            <i class="is-icon" :class="field.value"/>
                        </span>
                    </div>
                </template>
            </enso-form>
        </div>
    </div>
</template>

<script>
import { EnsoForm } from '@enso-ui/forms/bulma';

export default {
    name: 'Edit',

    components: { EnsoForm },
};
</script>
